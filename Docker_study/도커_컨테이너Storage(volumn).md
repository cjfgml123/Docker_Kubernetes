### Container Storage

#### 1. Volume 옵션

```dockerfile
# -v <host path>:<container mount path>
# -v <host path>:<container mount path>:<read write mode>
# -v <container mount path>

docker run -d -v /dbdata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=pass mysql

# ro : readonly 옵션으로 ex) 해커가 도커 컨테이너 저장소에 파일을 넣으면 host에도 생기므로 ro옵션을 줌.
docker run -d -v /webdata:/var/www/html:ro	httpd:latest

# 호스트에 임의의 디렉토리를 만들어서 자동으로 진행
# 우분트 기준 : /var/lib/docker/volume/{컨테이너ID}/_data
docker run -d -v /var/lib/mysql -e MYSQL..PASSWORD=pass mysql:latest

# 현재 디렉토리에 volume 마운트 하기 : ${pwd} 사용하기
docker run -d --rm --name web -p 80:80 -v ${pwd}:/usr/share/nginx/html:ro nginx
```

#### 2. 컨테이너 끼리 데이터 공유

- 호스트의 같은 디렉토리를 마운트 시키면 됨.

#### 3. 실습

- 컨테이너와 호스트 디렉토리를 마운트 후 컨테이너를 삭제해도 볼륨은 삭제 되지 않는다.

```dockerfile
docker volume ls # 볼륨 조회
docker volume rm [volume name] # 볼륨 삭제
```

#### 3-1) 웹데이터 readonly 서비스로 컨테이너에 지원하기

```dockerfile
# 1. echo "<h1>Hello</h1>" > index.html
# 2. cat index.html 로 출력 확인  # <h1>Hello</h1>
```
