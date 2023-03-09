# 쿠버네티스 Pod 와 Container

#### 1. 파드(Pod) 

- 컨테이너를 표현하는 K8S API의 최소 단위
- 하나 또는 여러 개의 컨테이너가 포함될 수 있음.

##### 1-1) Pod 생성하기 CLI

```powershell
# kubectl run 명령(CLI)으로 생성 , webserver : Pod이름
kubectl run webserver --image=nginx:1.14
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

