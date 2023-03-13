# livenessProbe를 이용해 Self-healing Pod

#### 1. Liveness Probe (kubelet에 의해 주기적으로 진단(diagnostic))

- 컨테이너의 실행 여부를 진단하여 실패 시 컨테이너를 종료시키고 재시작 정책에 따라 컨테이너 재시작(Pod를 재시작하는 것이 아니라 Container을 재시작하는 것.)

- 컨테이너가 재시작되더라도 Pod의 IP address는 바뀌지 않는다.

- Pod의 spec에 정의

- 상세링크 "https://kubernetes.io/ko/docs/concepts/workloads/pods/pod-lifecycle/"


##### 1-1) 일반적인 Pod-definition : self healing 기능 x

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx-container
    image: nginx:1.14
```

##### 1-2) livenessProbe definition : self healing 기능 o

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx-container
    image: nginx:1.14
    livenessProbe:
      httpGet:
        path: /
        port: 80
```



#### 2. Liveness Probe 매커니즘

##### 2-1) httpGet probe

- httpGet probe: 지정한 IP주소, port, path에 HTTP GET 요청을 보내, 해당 컨테이너가 응답하는지를 확인
- 반환코드가 200이상 400미만이면 진단 성공, 아닌 값이 나오면 오류. 컨테이너를 다시 시작함.

```yaml
livenessProbe:
  httpGet:
    path: /
    port: 80
```

##### 2-2) tcpSocket probe

- 지정된 포트에서 컨테이너의 IP주소에 대해 TCP검사를 수행한다. 포트가 활성화 되어 있다면 진단이 성공한 것으로 간주한다. 원격 시스템(컨테이너)가 연결을 연 이후 즉시 닫는다면, 이 또한 진단이 성공한 것으로 간주한다. 연결되지 않으면 컨테이너를 다시 시작한다.

```yaml
livenessProbe:
  tcpSocket:
    port: 22
```

##### 2-3) exec probe

- 컨테이너 내에서 exec 명령을 전달하고 명령의 종료코드가 0이 아니면 컨테이너를 다시 시작한다.

```yaml
livenessProbe:
  exec:
    command: # data가 있는지 확인 명령어
    - ls
    - /data/file
```



#### 3. Liveness probe 매개변수

| livenessProbe 필드                  | 설명                                     |
| ----------------------------------- | ---------------------------------------- |
| periodSeconds (default : 10초)      | health check 반복 실행 시간(초)          |
| initialDelaySeconds (default : 0초) | Pod 실행 후 delay할 시간(초)             |
| timeoutSeconds (default : 1초)      | health check 후 응답을 기다리는 시간(초) |
| successThreshold (default : 1번)    | 성공 횟수                                |
| failureThreshold (default : 3번)    | 연속 실패 횟수                           |

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx-container
    image: nginx:1.14
    livenessProbe:
      httpGet:
        path: /
        port: 80
        
      initialDelaySeconds: 15
      periodSeconds: 20
      timeoutSeconds: 1
      successThreshold: 1
      failureThreshold: 3
```

