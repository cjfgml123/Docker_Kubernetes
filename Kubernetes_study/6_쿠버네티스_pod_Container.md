# 쿠버네티스 Pod 와 Container

#### 1. 파드(Pod) 

- 컨테이너를 표현하는 K8S API의 최소 단위

- Pending 단계를 시작으로 기본 컨테이너 중 적어도 하나 이상이 OK로 시작하면 Running단계가 되고 파드의 컨테이너가 실패로 종료되었는지 여부에 따라 Succeeded or Failed 단계로 이동.

- 생성 시 고유ID(UID)가 할당되고, Pod의 노드가 종료되면, 해당 노드에 스케줄된 Pod는 타임아웃 기간 후에 삭제되도록 스케줄된다.

- Pod는 자체적으로 자가 치유되지 않는다.

- 파드가 노드에 스케줄된 후에 해당 노드가 실패하면, 파드는 삭제된다.

- UID로 정의된 특정 파드는 다른 노드로 절대 "다시 스케줄" 되지 않는다.

- 하나 또는 여러 개의 컨테이너가 포함될 수 있음.

  ```powershell
  # 동작중인 파드 정보 보기
  kubectl get pods
  kubectl get pods -o wide
  kubectl describe pod mypod 
  
  # 동작중인 파드 수정 : powerShell에서 직접 수정가능
  kubectl edit pod mypod 
  
  # READY : Pod에서 실행되는 컨테이너 개수 
  # STATUS : ErrImagePull or ImagePullBackOff 은 에러,Running이 정상동작
  # 에러 시 READY 0/1 , 1/2 이런 식으로 작동 
  (base) PS C:\Users\User> kubectl get pods
  NAME       READY   STATUS    RESTARTS   AGE
  multipod   2/2     Running   0          2m43s
  redis      1/1     Running   0          13m
  
  # pod 에러 잡기
  kubectl describe pod mypod
  # 후에 Events에러 log 확인 
  ```

##### 1-1) Pod 생성하기 CLI

```powershell
# kubectl run 명령(CLI)으로 생성 , webserver : Pod이름
kubectl run webserver --image=nginx:1.14

# 모든 pod 삭제
kubectl delete pod --all

# 개별 pod 삭제
kubectl delete pod mypod
```

##### 1-2) Pod 생성하기 yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webserver # Pod 이름
spec:
  containers:
  - name: nginx-container # 컨테이너 이름
    image: nginx:1.14
    imagePullPolicy: Always 
    ports:
    - containerPort: 80
      protocol: TCP
```

- yaml 파일 생성 후 

```powershell
kubectl create -f pod-nginx.yaml 
```

##### 1-3) Pod 동작 확인

```powershell
kubectl get pods
kubectl get pods -o wide
kubectl get pods -o yaml # 활용 : 동작중인 pod들을 yaml 형식으로 Copy할 수 있음.
kubectl get pods -o json
```

##### 1-4) Pod에 접속해서 결과보기

```powershell
#kubectl exec webserver -it -- /bin/bash 
#curl <pod's IP address>
```

##### 1-5) multi-container Pod 생성하기

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multipod # Pod 이름
spec:
  containers:
  - name: nginx-container # 컨테이너 이름
    image: nginx:1.14 
    ports:
    - containerPort: 80
  - name: centos-container # 컨테이너 이름
    image: centos:7 
    command:
    - sleep
    - "10000"
```

- yaml 파일 생성 후 

```power
kubectl create -f pod-multi.yaml
kubectl get pods
kubectl get pods -o wide
```

- 다중 컨테이너를 가진 pod에서 하나의 컨테이너 접속

```powershell
kubectl exec multipod -it -c centos-container -- /bin/bash
curl http://localhost:80
exit
```

- 다중 컨테이너를 가진 pod에서 컨테이너의 로그 출력

```powershell
kubectl logs multipod -c nginx-container

# single 컨테이너의 파드는
kubectl logs (podName) #으로 입력
```

##### 1-6) pod 동작과정 확인

- PowerShell 2개 준비

  ```powershell
  # 먼저 하나의 powershell에
  kubectl get pods -o wide --watch # 실행 pod 상태를 모니터링
  
  # 하나는 pod 생성 및 삭제 명령어
  kubectl create -f nginx.yaml 
  kubectl delete pod mypod # 후 상태 모니터링 
  ```

##### 1-7) PowerShell로 yaml 파일 생성

```powershell
# --dry:실행되는지 확인해줘
# -o yaml : yaml파일로 출력
kubectl run redis --image=redis --dry-run -o yaml
# yaml 파일로 저장
kubectl run redis --image=redis --dry-run -o yaml > redis.yaml
```



