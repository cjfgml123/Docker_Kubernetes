# 쿠버네티스 아키텍쳐

#### 1. 쿠버네티스 yaml 템플릿

- python처럼 들여쓰기로 데이터 계층을 표기
- 들여쓰기 Tab이 아닌 Space Bar 사용
- Scalar 문법 : ':'을 기준으로 key:value를 설정
- 배열 문법 : '-' 문자로 여러 개를 나열
- 공식 사이트 "http://yaml.org/"

##### 1-1) 쿠버네티스 yaml 파일 예제 - v1 버전의 Pod를 실행  

```yaml
apiVerion: v1 
kind: Pod # 어떤 종류의 오브젝트를 생성하고자 하는지
parent:
  child1: first child
  key2:
    child-1: kim
  key3:
    - grandchild1:
      name: kim
    - grandchild2:
      name: lee #com
#comment line 
```

 ```yaml
 # nginx yaml example
 apiVersion: v1
 kind: Pod
 metadata: # 문자열인 이름,UID, 선택적인 네임스페이스를 포함하여 오브젝트를 유일하게 구분지어 줄 데이터
   name: mypod
 spec: # 오브젝트에 대해 어떤 상태를 의도하는지
   containers:
    -  image: nginx
       name: nginx
       ports:
       -  containerPort: 80
       -  containerPort: 443
 ```

##### 1-2) 상세 예제

- apiVersion : kind의 종류에 따라 다름.
- kind : 리소스의 종류를 명시(ex: Pod, Service, ReplicaSet, Deployment 등)
- metadata : 리소스의 라벨, 이름 등을 지정
- specification : "spec"를 말함. 각 컴포넌트에 대한 상세 설명. 어떤 오브젝트 종류인지에 따라 다른 내용을 담는다.
- status : 쿠버네티스가 자동으로 생성, 자신이 원하는 상태가 되도록 현재 상태를 기술 
- labels : Pod와 같은 오브젝트에 첨부된 키와 값의 쌍이다. 오브젝트의 특성을 식별하는데 있어 사용자에게 중요하지만, 시스템에 직접적인 의미는 없다.
- Selectors : Label을 기반으로 오브젝트를 어떻게 선택하는지에 대한 방법의 표현 

```powershell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

##### 1-2) API version

- alpha -> beta -> stable 과정으로 version 업데이트
- kubernetes Object 정의 시 apiVersion이 필요

- API version 이 다르면 실행 불가 , 오브젝트에 맞는 api version 사용해야 함.
  - API Object의 종류 및 버전
    - Deployment : apps/v1
    - Pod : v1
    - ReplicaSet : apps/v1
    - ReplicationController : v1
    - Service : v1
    - PersistentVolume : v1 

```powershell
# 리소스 API version 출력 방법
# kubectl explane [오브젝트명] : 리소스의 정보(Documentation) 출력
kubectl explain pod
```

