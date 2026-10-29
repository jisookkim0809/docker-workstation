# 개발 워크스테이션 구축
<<<<<<< HEAD
 터미널(리눅스 CLI), Docker, Git/GitHub을 직접 세팅하고 검증한 기록입니다.
=======

터미널(리눅스 CLI), Docker, Git/GitHub을 직접 세팅하고 검증한 기록입니다.
>>>>>>> 1a202dd689a769b59c5ba87791180ead8628091e

## 1) 프로젝트 개요

- 목표: 터미널로 작업 환경을 정리하고, Docker로 웹 서버를 컨테이너화하여 포트 매핑·바인드 마운트·볼륨을 검증하고, Git/GitHub으로 결과물을 관리한다.
- 핵심 확인 사항: 이미지와 컨테이너의 분리, 격리된 실행 환경, 포트·스토리지 연결 방식

## 2) 실행 환경

| 항목 | 내용 |
|---|---|
| OS | Windows + WSL Ubuntu (버전: `lsb_release -d` 결과) |
| Shell / Terminal | bash / VS Code 터미널 (WSL: Ubuntu) |
| Docker | (`docker --version` 결과) |
| Git | (`git --version` 결과) |

```bash
$ lsb_release -d
(여기에 출력 붙여넣기)

$ docker --version
(여기에 출력 붙여넣기)

$ git --version
(여기에 출력 붙여넣기)
```

## 3) 수행 체크리스트

- [x] Git 사용자 정보/기본 브랜치 설정
- [x] GitHub 저장소 생성 및 `gh` 로그인, push 연동
- [x] 터미널 기본 조작 (이동/생성/복사/이름변경/이동/삭제)
- [x] 권한 변경 실습 (파일 1개, 디렉토리 1개)
- [ ] Docker 설치/점검 (`docker --version`, `docker info`)
- [ ] Docker 기본 운영 명령 (images, ps, logs, stats)
- [ ] hello-world / ubuntu 컨테이너 실행
- [ ] Dockerfile 기반 커스텀 이미지 빌드/실행
- [ ] 포트 매핑 접속 (8080, 8081)
- [ ] 바인드 마운트 반영 확인
- [ ] 볼륨 영속성 검증
- [ ] VS Code GitHub 연동 증거

## 4) 수행 로그 및 검증

### 4-1. Git 설정

```bash
$ git config --list
git config --list
user.email=jisookkim0809@gmail.com
user.name=jisookkim0809
```

### 4-2. GitHub 로그인 및 저장소 연동

```bash
$ gh auth status
github.com
  ✓ Logged in to github.com account jisookkim0809 (/home/jisookkim0809/.config/gh/hosts.yml)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************************************
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'

$ git push -u origin main
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 255 bytes | 255.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/jisookkim0809/docker-workstation.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

- 증거 이미지:

![gh auth status](images/gh-auth-status.png)
![git push](images/git-push.png)
![GitHub 저장소 화면](images/github-repo.png)

### 4-3. 터미널 기본 조작 로그

```bash
$ cd ~/codyssey/practice
$ pwd
/home/jisookkim0809/codyssey/practice

$ ls -la
total 16
drwxr-xr-x 3 jisookkim0809 jisookkim0809 4096 Oct 29 15:58 .
drwxr-xr-x 4 jisookkim0809 jisookkim0809 4096 Oct 29 15:47 ..
-rw-r--r-- 1 jisookkim0809 jisookkim0809    6 Oct 29 15:50 a.txt
-rw-r--r-- 1 jisookkim0809 jisookkim0809    0 Oct 29 15:49 empty.txt
drwxr-xr-x 2 jisookkim0809 jisookkim0809 4096 Oct 29 15:58 testdir

$ touch empty.txt
$ echo "hello" > a.txt
$ ls
a.txt  empty.txt  testdir

$ cat a.txt
hello

$ cp a.txt b.txt
$ ls
a.txt  b.txt  empty.txt  testdir

$ mv b.txt c.txt
$ mkdir dir1
$ mv c.txt dir1/
$ ls dir1
c.txt

$ rm dir1/c.txt
$ rmdir dir1
$ ls
a.txt  empty.txt  testdir
```

### 4-4. 권한 실습 (변경 전/후)

파일:

```bash
$ ls -l a.txt
(변경 전 출력, 예: -rw-r--r--)

$ chmod 600 a.txt
$ ls -l a.txt
(변경 후 출력, 예: -rw-------)

$ chmod 644 a.txt
$ ls -l a.txt
(원복 출력)
```

디렉토리:

```bash
$ ls -ld testdir
(변경 전 출력, 예: drwxr-xr-x)

$ chmod 700 testdir
$ ls -ld testdir
(변경 후 출력, 예: drwx------)

$ chmod 755 testdir
$ ls -ld testdir
(원복 출력)
```

### 4-5. Docker 설치 및 점검

```bash
$ docker --version
(출력)

$ docker info
(출력 - 길면 핵심 부분 발췌)
```

### 4-6. Docker 기본 운영 명령

```bash
)$ docker pull nginx:alpine
alpine: Pulling from library/nginx
b9b388a21ecf: Pull complete 
7b229517ed22: Pull complete 
26185d08a3ec: Pull complete 
12b85cff06fd: Pull complete 
7cc330898115: Pull complete 
657023555a2a: Pull complete 
2ec5fb117ffa: Pull complete 
9b0f7dfe35b2: Pull complete 
a4d5b1f57abc: Download complete 
acc2d54c3743: Download complete 
Digest: sha256:785835ea65db4bba6f021acc0f3be91b0fef76a53fe41f8588c6698d2aed0606
Status: Downloaded newer image for nginx:alpine
docker.io/library/nginx:alpine

$ docker images
                                                       i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       25.9kB         9.49kB    U   
nginx:alpine         785835ea65db       94.5MB         27.2MB        
ubuntu:latest        6a06111c3b0a        163MB         45.9MB    U   

$ docker run -d --name web nginx:alpine
88e4c7574ee2f750387e0537682931704796384673909341d7bea65c9df18f57

$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED                  STATUS                  PORTS     NAMES
88e4c7574ee2   nginx:alpine   "/docker-entrypoint.…"   Less than a second ago   Up Less than a second   80/tcp    web

$ docker logs web
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/10/29 12:15:34 [notice] 1#1: using the "epoll" event method
2026/10/29 12:15:34 [notice] 1#1: nginx/1.31.8
2026/10/29 12:15:34 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0) 
2026/10/29 12:15:34 [notice] 1#1: OS: Linux 6.18.54.1-microsoft-standard-WSL2
2026/10/29 12:15:34 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/10/29 12:15:34 [notice] 1#1: start worker processes
2026/10/29 12:15:34 [notice] 1#1: start worker process 30
2026/10/29 12:15:34 [notice] 1#1: start worker process 31
2026/10/29 12:15:34 [notice] 1#1: start worker process 32
2026/10/29 12:15:34 [notice] 1#1: start worker process 33
2026/10/29 12:15:34 [notice] 1#1: start worker process 34
2026/10/29 12:15:34 [notice] 1#1: start worker process 35
2026/10/29 12:15:34 [notice] 1#1: start worker process 36
2026/10/29 12:15:34 [notice] 1#1: start worker process 37

$ docker stats --no-stream
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O      BLOCK I/O     PIDS
88e4c7574ee2   web       0.00%     8.508MiB / 7.625GiB   0.11%     690B / 84B   0B / 8.19kB   9

$ docker stop web
web

$ docker rm web
web

$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
0b473335d3ad   ubuntu        "bash"     8 minutes ago    Exited (0) 7 minutes ago              my-ubuntu
e2eb6ce9947d   hello-world   "/hello"   48 minutes ago   Exited (0) 48 minutes ago             dazzling_jackson
```

### 4-7. 컨테이너 실행 실습

```bash
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
d5e71e642bf5: Download complete 
Digest: sha256:c567b231f65614688711a75f31518a064258a51d442117a3e771a3a8fdba78ef
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

$ docker run -it --name my-ubuntu ubuntu bash
(이미지 내려받기 로그 중략)
Digest: sha256:6a06111c3b0a8600feadb270aec771dc239bf407b25625f93f44ab00e11f4d3c
Status: Downloaded newer image for ubuntu:latest
root@0b473335d3ad:/# ls
bin   etc   lib64  opt   run   sys  var
boot  home  media  proc  sbin  tmp
dev   lib   mnt    root  srv   usr
root@0b473335d3ad:/# echo "hi from container"
hi from container
root@0b473335d3ad:/# exit
exit

$ docker start my-ubuntu
my-ubuntu

$ docker exec -it my-ubuntu bash
root@0b473335d3ad:/# echo "hi from exec"
hi from exec
root@0b473335d3ad:/# exit
exit

$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
0b473335d3ad   ubuntu    "bash"    12 minutes ago   Up 31 seconds             my-ubuntu

$ docker attach my-ubuntu
root@0b473335d3ad:/# echo "hi from attach"
hi from attach
root@0b473335d3ad:/# exit
exit

$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
0b473335d3ad   ubuntu        "bash"     13 minutes ago   Exited (0) 12 seconds ago             my-ubuntu
e2eb6ce9947d   hello-world   "/hello"   53 minutes ago   Exited (0) 53 minutes ago             dazzling_jackson
```

attach와 exec 차이 관찰 메모:

-  exec로 들어가 exit한 뒤에도 docker ps에 my-ubuntu가 Up 상태로 남아 있었다. exec는 실행 중인 컨테이너에 새 프로세스(bash)를 하나 더 띄우는 것이라, 그 프로세스만 끝나고 컨테이너는 유지된다.
- attach로 들어가 exit하면 docker ps가 비었고, docker ps -a에서 Exited (0)으로 바뀌었다. attach는 컨테이너가 처음 실행한 메인 프로세스(bash)에 직접 연결되는 것이라, 거기서 나오면 메인 프로세스가 끝나 컨테이너도 종료된다.

### 4-8. 커스텀 이미지 (Dockerfile)

- 선택한 베이스: `nginx:alpine`
- 커스텀 포인트와 목적:
  - `LABEL`: (목적)
  - `ENV APP_ENV=dev`: (목적)
  - `COPY app/ /usr/share/nginx/html/`: (목적)

```dockerfile
(Dockerfile 내용 붙여넣기)
```

```bash
$ docker build -t my-web:1.0 .
(출력)
```

### 4-9. 포트 매핑 접속

```bash
$ docker run -d -p 8080:80 --name my-web-8080 my-web:1.0
$ curl http://localhost:8080
(출력)

$ docker run -d -p 8081:80 --name my-web-8081 my-web:1.0
$ curl http://localhost:8081
(출력)
```

![localhost:8080 접속](images/port-8080.png)
![localhost:8081 접속](images/port-8081.png)

### 4-10. 바인드 마운트

```bash
(실행 명령 + 호스트 파일 변경 전/후 curl 결과)
```

### 4-11. Docker 볼륨 영속성

```bash
(볼륨 생성 → 컨테이너 연결 → 데이터 기록 → 컨테이너 삭제 → 새 컨테이너로 데이터 확인)
```

## 5) 개념 정리

- **절대 경로 vs 상대 경로:** (예시 포함 2~3줄)
- **파일 권한 r/w/x, 755/644:** (예시 포함 2~3줄)
- **포트 매핑이 필요한 이유:** (2~3줄)
- **Docker 볼륨:** (2~3줄)
- **Git vs GitHub:** (2~3줄)

## 6) 트러블슈팅 (2건 이상)

### 사례 1: sudo 비밀번호 인증 실패

- 문제: `sudo apt update` 실행 시 비밀번호 인증이 반복 실패
- 원인 가설: 설정한 비밀번호와 입력한 값이 다름
- 확인: (실제 확인한 내용)
- 해결: (실제 해결 방법)

### 사례 2: git push 시 인증 요구

- 문제: `git push` 시 Username/Password를 요구
- 원인 가설: WSL 안의 Git에 GitHub 인증 정보가 없음
- 확인: (실제 확인한 내용)
- 해결: `gh auth login`으로 브라우저 인증 후 push 성공

### 사례 3 (선택): `ls -1`과 `ls -l` 혼동

- 문제: 권한 확인 시 권한 정보가 출력되지 않음
- 원인 가설: 옵션이 숫자 1이었음
- 해결: 소문자 `l`로 다시 실행
