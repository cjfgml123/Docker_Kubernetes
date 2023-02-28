# 도커 Compose 사용하기

#### 1. 도커 Compose

- 여러 컨테이너를 일괄적으로 정의하고 실행할 수 있는 툴
- .yaml or .yml 확장자를 가진다.
- 여러 개의 명령어를 하나의 파일로 합쳐 실행 가능
- 파일 이름은 "docker-compose.yml" 로 사용(다른 이름을 사용할 때는 인자로 이름을 지정해줘야 함.)
- 한 폴더에 하나만 있을 수 있음.

  - 여러 개의 정의 파일을 사용하려면 그 개수만큼 폴더를 만들어야 함
  - 이미지 파일이나 HTML 파일도 컴포즈가 사용할 폴더에 함께 둔다.
- 컨테이너가 모인 것을 서비스라고 부름.
- https://docs.docker.com/ 에 옵션 설명 존재
- 윈도우와 / MAC 에서는 도커 툴박스나 Docker for windows,for Mac을 설치하면 도커 엔진과 함께 자동 설치됨.

  - 버전 확인 : docker-compose -v

#### 1-1) 도커 Compose 실행 순서

```dockerfile
# 1단계 : 서비스 디렉토리 생성
# mkdir webserver
# cd webserver

# 2단계 : 빌드를 위한 dockerfile 생성
# 3단계 : docker-compose.yml 생성
# 4단계 : docker-compose 명령어 실행
	docker-compose up -d
	docker-compose ps
	docker-compose scale mysql=2
	docker-compose ps
	docker-compose down 
```

#### 1-2) Compose 파일 작성법

- 첫 줄에 docker compose 버전 기재
- **‘주 항목 → 이름 추가 → 설정’** 순서로 작성

  - 주 항목
    - services, networks, volumes 등등
  - 주 항목 아래에 이름 추가 (공백으로 들여쓰기 필요)
    - 컨테이너 이름, 네트워크 이름, 볼륨 이름
    - 이름 뒤에는 반드시 **콜론(:)**을 붙여야 함
    - 해당 줄에 이어서 컨테이너 설정을 기재하려면 콜론 뒤로 공백이 하나 있어야 함
  - 설정
    - 기재할 내용이 한 가지라면 콜론 뒤에 이어서 작성 **(사이에 공백 필수)**
    - 내용이 여러 개라면 줄을 바꿔 하이픈(-)을 앞에 적고 들여쓰기를 맞춰야 함
    - 하이픈을 앞에 적은 행은 다시 공백을 넣어 들여 써줘야 함

#### 1-3) 도커 Compose 명령어

- 다른 폴더의 compose 파일 실행 시

  - docker-compose -f /other-dir/docker-compose.yml  , -f 실행
- 정의 파일(compose file)

```
up : 컨테이너 생성/시작
	- 정의 파일에 기재된 내용대로 이미지를 내려받고 컨테이너를 생성 및 실행
	- 정의 파일에는 네트워크나 볼륨에 대한 정의도 기재할 수 있어서 주변 환경을 한번에 생성 가능
	- 보통 -d 옵션을 사용하여 백그라운드에서 실행, 이 옵션을 사용하지 않으면 현재 터미널이 컨테이너의 로		  그가 출력되고 "Ctrl + c"를 눌러서 탈출하는 순간 컨테이너가 모두 정지됨.
ps : 컨테이너 목록 표시

logs : 컨테이너 로그 출력

run : 서비스 컨테이너의 특정 명령어를 일회성으로 실행할 때 사용, 실행중인 컨테이너에 명령어 전달
	- ex) docker-compose run web env # 환경변수 출력됨.

start : 컨테이너 시작

scale : 컨테이너 지정개수 만큼 생성 ex) docker-compose scale "컨테이너이름"=2 

stop : 컨테이너 정지

restart : 컨테이너 재시작

pause : 컨테이너 일시 정지

unpause 컨테이너 재개

port : 공개 포트 번호 표시

config : 구성확인,compose 파일 문법 체킹

kill : 실행 중인 컨테이너 강제 정지

rm : 컨테이너 삭제

down : 리소스 삭제
	- 컨테이너와 네트워크를 정지 및 삭제
	- 불륨과 이미지는 삭제하지 않음.
	- 컨테이너와 네트워크 삭제 없이 종료만 하고 싶다면 stop 명령어 사용.
```

#### 1-4) 컴포즈 파일 항목

| 항목        | docker run 커맨드의 해당 옵션 또는 인자 | 내용                                                                                                    |
| ----------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| version     | -                                       | 도커 컴포즈의 버전을 기재한다. 버전에 따라 지원 문법 다름. ex) version: "3"                             |
| services    | -                                       | 컨테이너를 정의한다.**(컨테이너의 집합체를 주로 서비스라고 한다.)**                               |
| build       |                                         | 컨테이너 빌드                                                                                           |
| image       | 이미지 인자                             | 사용할 이미지를 지정                                                                                    |
| networks    | —net                                   | 접속할 네트워크를 지정                                                                                  |
| command     |                                         | 컨테이너에서 실행될 명령어 지정                                                                         |
| volumes     | -v, —mount                             | 스토리지 마운트(볼륨)를 설정                                                                            |
| ports       | -p                                      | 포트 설정                                                                                               |
| link        |                                         | 다른 컨테이너와 연계할 때 연계할 컨테이너 지정, a라는 컨테이너가 b컨테이너의 환경변수를 알아야할때 사용 |
| expose      |                                         | 링크로 연계된 컨테이너에게만 공개할 포트                                                                |
| environment | -e                                      | 환경변수 설정                                                                                           |
| depends_on  | 없음                                    | 다른 서비스에 대한 의존관계를 정의(실행 순서,연동할때 사용)                                             |
| restart     | 없음                                    | 컨테이너 종료 시 재시작 여부를 설정, [no,always,on-failure] 옵션 존재                                   |

```yaml
version: "3"
services:
	webserver:
		image: nginx
		command: sh -c "yarn install && yarn run dev"
		port: 
		 - 80 # 앞에 포트 없는건 local에 랜덤 포트 부여
		 - 0.0.0.0:8443:433
		link:
		 db:mysql
		volumes:
		 - .:/var/www/html
	db:
		image: redis
		depends_on:
		  - webserver # 웹서버가 먼저 실행되고 db 수행
		environment: 
		  - MYSQL_ROOT_PASSWORD: pass
```

- ex. penguin 컨테이너의 정의에 ‘depends_on: -namgeuk” 내용이 포함됐다면 namgeuk 컨테이너를 생성한 다음에 penguin 컨테이너를 만듦
- volumes : https://joont92.github.io/docker/volume-container-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0/ 참고 (공부 예정 )

#### 1-5) Volume 컨테이너

- 데이터를 저장하는 것이 목적인 컨테이너

```yaml
version: "3"
services:
  db:
    image: mysql
    volumes:
      - db_data:/var/lib/mysql  # /var/lib/docker/volumes/ 여기에 저장됨.
    restart:always
    environment:
	MYSQL_ROOT_PASSWORD: somewordpress
	MYSQL_DATABASE: wordpress
	MYSQL_USER: wordpress
	MYSQL_PASSWORD: wordpress
  wordpress:
    depends_on:
	- db
    image: wordpress:latest
    volumes:
	- wordpress_data:/var/www/html  # /var/lib/docker/volumes/ 여기에 저장됨.
    ports:
        - "80:80"
    restart: always
    environment:
	WORDPRESS_DB_HOST: db:3306
	WORDPRESS_DB_USER: wordpress
	WORDPRESS_DB_PASSWORD: wordpress
	WORDPRESS_DB_NAME: wordpress
   volumes:
     db_data: {}
     wordpress_data: {}

```

#### - 참고 : Docker compose vs Dockerfile script vs K8s

- Docker compose : docker run 명령어를 여러 개 모아놓은 것과 같으며, 컨테이너와 주변 환경(네트워크, 볼륨 등) 을 한번에 생성할 수 있다.
- Dockerfile script : 컨테이너가 아닌 이미지를 만들기 위한 도구로, 네트워크나 볼륨같은 컨테이너 주변 환경들은 생성이 불가능하다.
- k8s :  도커 컨테이너를 관리하는 도구로 도커 컴포즈와 혼동하기 쉬우나 사용 목적 자체가 다르다. 도커 컴포즈로 컨테이너 관리는 불가능하다.
