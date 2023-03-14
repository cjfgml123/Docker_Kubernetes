# Pod - Init Container & Infra Container

#### 1. init Container를 적용한 Pod

- 앱 컨테이너 실행 전에 미리 동작시킬 컨테이너
- 본 Container가 실행되기 전에 사전 작업이 필요할 경우 사용
- Init Container가 모두 실행된 후에 앱 컨테이너를 실행
- 초기화 컨테이너는 앱 이미지에는 없는 유틸리티 또는 설정 스크립트 등을 포함할 수 있다.
- 초기화 컨테이너는 lifecycle, livenessProbe, readinessProbe, startupProbe를 지원하지 않는다. 왜냐하면 초기화 컨테이너는 파드가 준비 상태가 되기 전에 완료를 목표로 실행되어야 하기 때문.
- "https://kubernetes.io/ko/docs/concepts/workloads/pods/init-containers/"

##### 1-1) 예제

```powershell
# 다른 콘솔창
kubectl get pods -o wide --watch
```



- 두 개의 초기화 컨테이너를 포함한 예제 , 첫 번째는 myservice를 기다리고 두 번째는 mydb를 기다린다. 두 컨테이너들이 완료되면, 파드가 시작될 것이다.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app.kubernetes.io/name: MyApp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup myservice.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for myservice; sleep 2; done"]
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup mydb.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for mydb; sleep 2; done"]
```

```powershell
kubectl create -f myapp.yaml
```

- pod Running 안됨 

```yaml
# myservice yaml 생성 후 실행 
apiVersion: v1
kind: Service
metadata:
  name: myservice
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
```

- 초기화 컨테이너 2개 중 1개 실행 확인

```yaml
# yaml 생성 후 실행 
apiVersion: v1
kind: Service
metadata:
  name: mydb
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9377
```

- 초기화 컨테이너 2개 정상 동작으로 pod 실행됨. 

#### 2. Infra Container(pause) 

- pod 생성될때 자동으로 생성되는 컨테이너 
- pod에 대한 환경 ex : ip, host 등을 관리하는 컨테이너
