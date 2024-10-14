# Jenkins 환경 구성

## Jenkins 가 설치되는 노드에 docker 설치

[Rocky9 에 podman 대신 Docker 설치](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-rocky-linux-9)

## Jenkins 설치

[Jenkins 설치](https://www.jenkins.io/doc/book/installing/kubernetes/)를 참조하여 순서대로 진행  

```bash
# 변경한 내용
1. namespace
  - devops-tools -> jenkins
2. 설치할 노드 지정
  - pvc 와 deployment 에 설치할 노드 지정
3. 설치
  - kubectl create -f sv.yaml
  - kubectl create -f pvc.yaml
  - kubectl create -f jenkins.yaml
  - kubectl create -f service.yaml
4. ingress 연동
  - ingress 에 인증서 등록
  - https://jenkins.drimsys.net/  jenkins-service
5. 권장 Plugin 설치
6. Jeknins 관리자 생성
7. 추가 Plugin
  - Pipeline: Stage View
```

## docker 설정

```bash
1. docker 플러그인 설치
  - Docker API Plugin
  - Docker Commons Plugin
  - Docker Pipeline
  - Docker plugin
```

## kubenetes 설정

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
