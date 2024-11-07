# Jenkins 환경 구성

## Jenkins 가 설치되는 노드에 docker 설치

[Rocky9 에 podman 대신 Docker 설치](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-rocky-linux-9)

## Jenkins 설치

[Jenkins 설치](https://www.jenkins.io/doc/book/installing/kubernetes/)를 참조하여 순서대로 진행  

```bash
# rancher 적용시 변경한 내용
1. Jenkins 를 설치할 노드의 label 지정
   - jenkins: 'yes'
2. https://www.jenkins.io/doc/book/installing/kubernetes/ 의 4개 yaml 생성
   - 예제에 설정된 namespace 변경
      - devops-tools -> jenkins
   - 설치할 노드 지정
      - deployment.yaml 의 nodeSelector 를 통해 설치할 노드 지정
3. yaml 설치
  - kubectl create -f serviceAccount.yaml
  - kubectl create -f volume.yaml
  - kubectl create -f deployment.yaml
  - kubectl create -f service.yaml
4. ingress 연동
  - ingress 에 인증서 등록
  - URL (https://jenkins.drimsys.net/) 을 jenkins-service 앱으로 등록
```

serviceAccount.yaml

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: jenkins-admin
rules:
  - apiGroups: [""]
    resources: ["*"]
    verbs: ["*"]
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins-admin
  namespace: jenkins
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: jenkins-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: jenkins-admin
subjects:
- kind: ServiceAccount
  name: jenkins-admin
  namespace: jenkins
```

volume.yaml

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: jenkins-pv-volume
  labels:
    type: local
spec:
  storageClassName: local-storage
  claimRef:
    name: jenkins-pv-claim
    namespace: jenkins
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  local:
    path: /mnt
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - worker-node01
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: jenkins-pv-claim
  namespace: jenkins
spec:
  storageClassName: local-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins
  namespace: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins-server
  template:
    metadata:
      labels:
        app: jenkins-server
    spec:
      nodeSelector:
        jenkins: 'yes'
      securityContext:
            fsGroup: 1000
            runAsUser: 1000
      serviceAccountName: jenkins-admin
      containers:
        - name: jenkins
          image: jenkins/jenkins:lts
          resources:
            limits:
              memory: "2Gi"
              cpu: "1000m"
            requests:
              memory: "500Mi"
              cpu: "500m"
          ports:
            - name: httpport
              containerPort: 8080
            - name: jnlpport
              containerPort: 50000
          livenessProbe:
            httpGet:
              path: "/login"
              port: 8080
            initialDelaySeconds: 90
            periodSeconds: 10
            timeoutSeconds: 5
            failureThreshold: 5
          readinessProbe:
            httpGet:
              path: "/login"
              port: 8080
            initialDelaySeconds: 60
            periodSeconds: 10
            timeoutSeconds: 5
            failureThreshold: 3
          volumeMounts:
            - name: jenkins-data
              mountPath: /var/jenkins_home
      volumes:
        - name: jenkins-data
          persistentVolumeClaim:
              claimName: jenkins-pv-claim
```

service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: jenkins-service
  namespace: jenkins
  annotations:
      prometheus.io/scrape: 'true'
      prometheus.io/path:   /
      prometheus.io/port:   '8080'
spec:
  selector:
    app: jenkins-server
  type: NodePort
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 32000
```

## Jenkins 실행 및 설정

### 최초 실행 및 로그인

1. 권장 Plugin 설치
2. Jeknins 관리자 생성
3. 추가 Plugin
   - Pipeline: Stage View

### docker 환경 설정

```bash
1. docker 플러그인 설치
  - Docker API Plugin
  - Docker Commons Plugin
  - Docker Pipeline
  - Docker plugin
```

### kubenetes 환경 설정

```bash
1. kubenetes 플러그인 설치
2. Dashboard > Jenkins 관리 > Clouds > New Cloud > Kubernetes 생성
3. 설정
   - Kubernetes Namespace : jenkins
   - Credentials 생성
     - rancher 의 Cluster Management > Clusters > ... > Download KubeConfig
     - Add > Jenkins > Global... > Kind (Secret file) > upload
  - WebSocket
  - Jenkins URL : http://jenkins.drimsys.net
  - Pod Labels : jenkins yes
```

## Jenkins pipeline 설정

```jenkins
1. podTemplate 에 docker 가 실행할 node 를 nodeSelector 를 통해 지정
```

Jenkinsfile 예제

```Jenkinsfile
// 현재 빌드하는 파드의 이름
def label = "template_go_app-${BUILD_NUMBER}"
podTemplate(
		label: label,
		containers: [
			containerTemplate(name: "docker", image: "docker", command: "cat", ttyEnabled: true, privileged: true),
			containerTemplate(name: "kubectl", image: "lachlanevenson/k8s-kubectl", command: "cat", ttyEnabled: true),
		],
		nodeSelector: "jenkins=yes",
		// 현재 파드 내부에서 docker를 사용하기 위한 볼륨 마운트
		volumes: [
			hostPathVolume(hostPath: "/var/run/docker.sock", mountPath: "/var/run/docker.sock"),
		]
)
{
	node(label) {
		stage('Git Pull') {
			checkout scm
		}

		stage('Docker Build') {
			container("docker") {
				dockerApp = docker.build("drimcore/template_go_app", "--no-cache -f build/Dockerfile .")
			}
		}

		stage('Docker Push') {
			def VER = readFile 'build/version.txt'
			VER = VER.trim()
			def PATCH = readFile 'build/version-patch.txt'
			PATCH = PATCH.trim()
			def VERSION = "${VER}.${PATCH}"
			
			container("docker") {
				docker.withRegistry("https://harbor.drimsys.net", 'harbor-credential') {
					dockerApp.push("${VERSION}")
					dockerApp.push("latest")
				}
			}
		}

		stage('Kubernetes Apply POD') {
			container("kubectl") {
				sh "kubectl delete -f build/kube-deploy.yaml"
				sh "kubectl create -f build/kube-deploy.yaml"
			}
		}
	}
}

```
