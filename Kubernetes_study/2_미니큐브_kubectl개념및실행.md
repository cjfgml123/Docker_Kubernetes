# 미니큐브와 Kubectl 개념 및 실행

#### 1. 개념

- Minikube는 가벼운 쿠버네티스 구현체이며, 로컬 머신에 VM을 만들고 하나의 노드로 구성된 간단한 클러스터를 생성한다.
- Minikube CLI는 클러스터에 대해 시작, 중지, 상태 조회 및 삭제 등의 기본적인 부트스트래핑(bootstrapping) 기능을 제공한다.
- "https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/create-cluster/cluster-interactive/" 간단한 실습 제공

```powershell
# minikube 시작하기
minikube start 

# minikube 버전조회
minikube version


```

##### 1 - 2. 미니큐브 참고 명령어

```powershell
# 미니큐브에서 쿠버네티스 대시보드 실행, 터미널 하나 더 생성해서 실행하기 
# 대시보드 참고 자료 "https://kubernetes.io/ko/docs/tutorials/hello-minikube/"
minikube dashboard

```

#### 2. kubectl

- 쿠버네티스 클러스터와 통신하기 위한 커맨드 라인 도구

  - PowerShell 용 kubectl 자동완성 기능 설정 "https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-windows/"
  - ```powershell
    kubectl completion powershell | Out-String | Invoke-Expression
    ```

##### 2 - 1) kubectl 명령어 기본구조

```powershell
# kubectl [command] [type] [name] [flags] # 한칸씩 빈칸으로 구분, 생략 가능
# command : 자원(object)에 실행할 명령 ex) create, get, delete, edit ...
# type : 자원의 타입 ex) node, pod, service ..
# name : 자원의 이름 ex) 사용자 지정
# flags : 부가적으로 설정할 옵션 ex) --help, -o options , ...
kubectl get pod webserver -o wide
# 웹 서버 pod를 자세하게 조회
```

##### 2 - 2) kubectl 명령어

```powershell
# kubectl 설치 확인
  # 클라이언트 버전은 kubectl 버전이고, 서버 버전은 마스터에 설치된 kubernetes 버전입니다. 
kubectl version

# 클러스터 세부 정보 조회
kubectl cluster-info

# 클러스터 노드 조회
kubectl get nodes

# 파드를 관리할 디플로이먼트 생성, Docker 이미지를 기반으로 한 컨테이너 실행
kubectl create deployment hello-node --image=registry.k8s.io/echoserver:1.4

# 디플로이먼트 조회
kubectl get deployment

# 파드 조회
kubectl get pods

# 파드 상세 조회
kubectl get pods -o wide # or kubectl get pod (pod 이름) -o wide 

# webserver 라는 pod 생성
kubectl run webserver --image=nginx --port 80 

# 클러스터 이벤트 조회
kubectl get events

# kubectl 환경설정 조회
kubectl config view 

# object 삭제
kubectl delete pod webserver
```

##### 2 -3) kubectl 참고 명령어

```powershell
# 쿠버네티스 resource 명령어 약어 및 종류 조회
kubectl api-resources # ex) namespaces : ns , pods : po ..

# kubectl 명령어 조회
kubectl --help

# kubectl [command] --help #command에 대한 사용법 조회
kubectl logs --help 

# 컨테이너 내부 접속
kubectl exec webserver -it -- /bin/bash
```
