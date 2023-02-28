# 도커 private 레지스트리 구현 및 사용

#### 1. 도커  private 레지스트리 만들기

- private registry port : 5000 사용
- 이미지명은 registry 버전은 2로 사용, "docker hub"에서 registry:2 다운받기
- "--restart" : 도커 엔진 혹은 컨테이너가 종료 되었을 때 재시작의 정책을 말하며 always로 설정하면 내가 docker stop으로 정지시키지 않는 한 조건에 따라 차이가 있을 수 있지만 웬만한 비정상 종료 시엔 다시 컨테이너를 재시작 해준다.
- 일반적으로 registry 버전은 0점대 2점대가 있다.

```dockerfile
docker run -d -p 5000:5000 --restart=always --name docker-registry registry:2
```

#### 2. push할 image 설정

- 이미지 형식을 변경 해줘야 함.

```dockerfile
docker tag "기존 이미지" localhost:5000/"push할 이미지명"
docker push localhost:5000/"push할 이미지명"
```

#### 3. 테스트

```dockerfile
# [기존이미지, push할 이미지명] 이미지 삭제
docker rmi "기존이미지"
docker rmi "push할 이미지명"

# private 레지스트리에서 pull 받기
docker pull localhost:5000/"push한 이미지명"
```

#### 참고(일반적으로 이미지의 이름 형식)

- public : userID/imageName
- private : reg.test.com:5000/imageName or 192.168.1.101:5000/imageName 처럼 앞에 주소가 있음
- local : imageName/tag 처럼 이미지 이름과 태그만 입력
- 위의 형식은 같은IP대역 내에서도 일부 원격 사설 레지스트리 컨테이너에 접근할 수 있어 보안이 취약, 이를 위해 인증서를 활용하여 정해준 머신들만 이용할 수 있게 한다.
- Private Registry의 단점 중 하나는 어떤 이미지가 올라가 있는지 확인하기 번거로움이 있다. 이를 위해 누군가가 WEB GUI로 Private Registry를 볼 수 있는 이미지를 만들었다. (나중에 할 예정)
