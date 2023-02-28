# 도커 서비스 구축 실습

### 1. 도커 MySQL 서비스 구축 (환경변수 설정)

```dockerfile
docker run --name ms -e MYSQL_ROOT_PASSWORD=chlee -d --rm mysql
# "-e" : 환경변수 설정 , "MYSQL_ROOT_PASSWORD" mysql에서 정의한 환경변수명
# "--rm" : 컨테이너 일회성으로 실행할때 사용, 컨테이너 종료 시 관련된 리소스까지 제거

# "-it" : -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션(컨테이너의 표준 입력과 로컬 컴퓨터의 키보드 입력을 연결)
docker exec -it ms mysql -u root -p
# Enter password : 위의 설정한 패스워드 입력
# 성공 시 mysql 접속
show databases;
exit
```

### 2. 볼륨 마운트하여 Jupyter LAB 서비스 구축

#### 2-1) 볼륨 마운트 옵션 사용해 로컬 파일 공유하기

- 마운트 개념 : 물리적인 장치를 특정한 위치에 연결시켜 주는 과정을 마운트
  - ex) : 물리적인 장치와 a 라는 디렉토리가 마운트하고 데이터가 a에 저장되면 실제 해당 데이터는 물리적인 장치에 저장되어 있다.

```dockerfile
docker run -v <호스트 경로>:<컨테이너 내 경로>:<권한>
# /tmp:home/user:ro
```

- 권한의 종류 
  - ro  : 읽기 전용
  - rw : 읽기 및 쓰기 

```dockerfile
# 로컬 경로에 테스트용 index.html 만들기
<html>
<body>
Hello NginX with Docker!
</body>
</html>
```

```dockerfile
 docker run -d -p 90:80 --rm -v C:\Users\User\Desktop\Docker및쿠버네티스\LocalDir:/usr/share/nginx/html:ro nginx
 # http://localhost:90/ 접속
```

#### 2-2) Jupyter LAB 서비스 구축

```dockerfile
docker run --rm -p 8080:8888 -e JUPYTER_ENABLE_LAB=yes -v C:\Users\User\jupyternotebook:/home/jovyan/work:rw jupyter/datascience-notebook:latest
# or 
docker run --rm -p 8080:8888 -e JUPYTER_ENABLE_LAB=yes -v pwd:/home/jovyan/work:rw jupyter/datascience-notebook:latest
# pwd : window PowerShell에서 현재 디렉토리를 가져옴.
```



