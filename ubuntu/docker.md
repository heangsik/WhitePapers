# Ubuntu 24.04 - Docker Installation Guide

## 1. 시스템 업데이트

시스템 패키지 목록을 최신 상태로 업데이트

```bash
sudo apt update
```

## 2. 필수 패키지 설치

Docker를 설치하기 전에 필요한 몇 가지 패키지를 설치

```bash
sudo apt install ca-certificates curl gnupg lsb-release
```

## 3. Docker GPG 키 추가

Docker 패키지의 인증을 위해 GPG 키를 추가.

```bash
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

## 4. Docker 저장소 설정

Docker의 공식 저장소를 추가.

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 5. Docker 설치

이제 Docker 엔진과 관련 패키지를 설치.

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 6. Docker 서비스 시작 및 확인

Docker를 설치한 후 서비스를 시작하고 잘 작동하는지 확인.

```bash
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```

## 7. 사용자 권한 설정 (옵션)

`sudo` 없이 Docker 명령을 실행하려면 현재 사용자를 `docker` 그룹에 추가.

```bash
sudo usermod -aG docker $USER
```

변경 사항을 적용하려면 로그아웃 후 다시 로그인하거나 다음 명령어로 현재 셸을 갱신.

```bash
newgrp docker
```

## 8. Docker 설치 확인

정상적으로 설치되었는지 확인.

```bash
docker --version
docker run hello-world
```

# Docker 유용한 명령어

## 1. Docker 기본 명령어

- **`docker --version`**: Docker의 버전을 확인.
- **`docker info`**: Docker 데몬의 상태와 시스템 정보를 확인.
- **`docker help`**: Docker의 명령어 목록과 설명을 확인.

## 2. 이미지 관련 명령어

- **`docker images`**: 로컬에 저장된 Docker 이미지를 목록으로 보여준다.
- **`docker pull [이미지:태그]`**: Docker Hub나 다른 레지스트리에서 이미지를 다운로드.
  - 예: `docker pull ubuntu:latest`
- **`docker build -t [이미지:태그] [경로]`**: Dockerfile을 기반으로 이미지를 빌드.
  - 예: `docker build -t myapp:1.0 ./myapp`
- **`docker rmi [이미지:태그 또는 이미지ID]`**: Docker 이미지 삭제.
  - 예: `docker rmi ubuntu:latest`

## 3. 컨테이너 관련 명령어

- **`docker run [옵션] [이미지]`**: 새로운 컨테이너를 실행.
  - 예: `docker run -it ubuntu /bin/bash` (Ubuntu 컨테이너에서 Bash 셸을 실행)
  - 주요 옵션:
    - `-d`: 백그라운드에서 실행
    - `-p [호스트포트]:[컨테이너포트]`: 포트 매핑
    - `--name [컨테이너이름]`: 컨테이너에 이름 지정
    - `-v [호스트디렉토리]:[컨테이너디렉토리]`: 볼륨 마운트
- **`docker ps`**: 실행 중인 컨테이너 목록을 보여준다.
- **`docker ps -a`**: 실행 중이거나 종료된 모든 컨테이너 목록을 보여준다.
- **`docker stop [컨테이너ID 또는 이름]`**: 실행 중인 컨테이너를 중지.
  - 예: `docker stop myapp`
- **`docker start [컨테이너ID 또는 이름]`**: 중지된 컨테이너를 다시 시작.
- **`docker rm [컨테이너ID 또는 이름]`**: 중지된 컨테이너를 삭제.
  - 예: `docker rm myapp`
- **`docker exec -it [컨테이너ID 또는 이름] [명령어]`**: 실행 중인 컨테이너 내부에서 명령어를 실행.
  - 예: `docker exec -it myapp /bin/bash`

## 4. 네트워크 관련 명령어

- **`docker network ls`**: Docker 네트워크 목록을 확인.
- **`docker network create [네트워크이름]`**: 새로운 네트워크를 생성.
- **`docker network connect [네트워크이름] [컨테이너ID 또는 이름]`**: 컨테이너를 네트워크에 연결.
- **`docker network disconnect [네트워크이름] [컨테이너ID 또는 이름]`**: 컨테이너를 네트워크에서 분리.

## 5. 로그 및 상태 확인

- **`docker logs [컨테이너ID 또는 이름]`**: 컨테이너의 로그를 출력.
  - 예: `docker logs myapp`
- **`docker inspect [컨테이너ID 또는 이름]`**: 컨테이너나 이미지의 상세 정보를 JSON 형식으로 확인.
- **`docker stats [컨테이너ID 또는 이름]`**: 컨테이너의 리소스 사용량을 실시간으로 보여준다.

## 6. 볼륨 관련 명령어

- **`docker volume ls`**: Docker 볼륨 목록을 확인.
- **`docker volume create [볼륨이름]`**: 새로운 볼륨을 생성.
- **`docker volume rm [볼륨이름]`**: 볼륨을 삭제.

## 7. 이미지 및 컨테이너 정리

- **`docker system prune`**: 사용하지 않는 컨테이너, 이미지, 네트워크 등을 한꺼번에 정리.
- **`docker image prune`**: 사용하지 않는 이미지를 정리.
- **`docker container prune`**: 중지된 컨테이너를 삭제.
