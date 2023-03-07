# 쿠버네티스 아키텍쳐

#### 1. 쿠버네티스 namespace

- K8S API 종류 중 하나로서 클러스터 하나를 여러 개의 논리적인 단위로 나눠사 사용
- 쿠버네티스 클러스터 하나를 여러 팀이나 사용자가 함께 공유
- 용도에 따라 실행해야 하는 앱을 구분할 때 사용 
- ex) 배포용, 베타용, 개발용 각각의 namespace 생성
- 기본적으로 "default" namespace 사용

##### 1-1) namespace 사용하기

- namespace 생성

```powershell
 #1. CLI 방법 , blue : namespace 이름
 kubectl create namespace blue
 kubectl get namespaces
 
 #2. yaml 생성해서 만드는 방법
 # "kubectl create namespace green --dry-run -o yaml" green이라는 namespace를 만드는 yaml파일을 출력
 kubectl create namespace green --dry-run -o yaml > green-ns.yaml
 vim green-ns.yaml
 kubectl create -f green-ns.yaml
```

- namespace 관리

```powershell
# namespace 줄여서 -n으로 표기 가능
kubectl get namespaces # 기본적으로 4개의 namespace 출력됨.
kubectl delete namespace
```

- 특정 namespace에 pod 만들기 

```powershell
# 방법 1.
kubectl create -f nginx.yaml -n blue # -n = namespace

# 방법 2. 
# .yaml 파일에 metadata:에 namespace:blue 작성 후 
kubectl create -f nginx.yaml # 실행
```

- 사용할 namespace switch

```powershell
# 기본으로 사용하는 namespace를 default가 아닌 다른 namespace로 switch

# 방법1. namespace를 포함한 context 등록
kubectl config --help
# --cluster와 --user, --namespace를 쓰는 blue@kuber 생성
kubectl config set-context blue@kuber --cluster=kubernetes --user=kubernetes-admin --namespace=blue 
kubectl config view # 생성확인

# 현재 context 확인
kubectl config current-context

#등록된 namespace로 context 변경 
kubectl config use-context blue@kuber
```

