# Docker

## 이미지 만들기

- 프로젝터를 Build한다.
  > vite build
- Dockerfile을 프로젝트 Root에 생성한다.

  ```docker
    # 기본이되는 도커 이미지
    # 개발 시에 사용된 버젼을 잘 확인해야 함
    FROM node:20.17.0

    # 프로젝트가 설치될 위치
    WORKDIR /usr/src/app

    # node에서 필요한 모듈을 설치하기 위한 파일을 복사
    COPY package*.json ./

    # 필요 모듈 설치
    RUN npm i

    # 빌드가 완료된 경로
    COPY ./build ./build


    # 실행 명령어
    CMD ["node", "build"]
  ```

- 이미지 생성
  > docker build -t {이미지 이름}:{tag} .
- 컨테이너 실행
  > docker run -d -p 3000:3000 --name bus-stop docker-sample:0.1
  - -d : 백그라운드로 실행
  - -p 3000(a):3000(b) : 포트포워딩 옵션 a-외부 포트, b-내부포트
- Docker Compose

  - docker-compose.yml 파일 생성

  ```dc

  ```
