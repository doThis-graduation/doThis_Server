# 졸업 작품 [doThis] Server

- [English](README.en.md)
- [한국어](README.md)

## 소개

**Flask 기반의 운동 영상 분석 서버**는 사용자의 운동 영상을 분석하여 의미 있는 피드백을 제공하기 위해 설계되었습니다. Flask 프레임워크 위에 구축된 이 서버는 간편하고 직관적인 경험을 제공하여, 운동 애호가 및 트레이너가 자신의 운동 루틴에 대해 유용한 통찰을 얻을 수 있도록 돕습니다.

우리 서버는 **컴퓨터 비전 기술**을 활용해 영상에서 핵심 정보를 추출합니다. 개인의 훈련 최적화는 물론, 전문 트레이너가 맞춤형 피드백을 제공하는 데에도 사용할 수 있습니다.

---

## 주요 기능

- **영상 분석**

    컴퓨터 비전 알고리즘을 통해 운동 영상을 분석하여, 신체 움직임, 운동 자세 및 자세의 일관성 등 유용한 정보를 추출합니다. 이를 통해 사용자는 자신의 자세를 이해하고 개선할 수 있습니다.

- **자세 교정**

    운동 시 바른 자세를 유지하도록 피드백을 제공합니다. 올바르지 않은 자세를 감지하고, 교정 및 정렬을 위한 권장사항을 전달하여 안전하고 효과적인 운동을 유도합니다.

- **확장성 및 성능**

    Flask 기반 서버는 많은 영상 분석 요청도 안정적으로 처리할 수 있도록 설계되었습니다. 멀티 스레딩 기술을 사용하여 사용자가 분석 결과를 지연 없이 확인할 수 있도록 성능을 보장합니다.

- **Docker 볼륨을 이용한 안정성 확보**

    임시 DB로 사용할 Docker 볼륨을 생성하여 데이터를 로컬에 저장하지 않고 관리합니다. 서버 실행 중 사용자 영상 데이터를 변수에 저장하면 RAM 사용량이 증가해 서버에 부담을 줄 수 있으며, Firebase 업로드 전에 서버가 종료되면 데이터가 유실될 수 있습니다. 이를 방지하기 위해 Docker 볼륨을 활용하여 안정성을 강화했습니다.

---

## 서버 아키텍처

**클라이언트가 영상을 업로드하고 분석 결과를 받기까지의 흐름은 다음과 같습니다.**

![Untitled](https://github.com/CHOHYUNSIK/Flask-docker/assets/69946205/0029b116-16cf-4602-9834-3f8e3228de8f)

**[설명]**

1. 클라이언트가 운동 영상을 데이터베이스에 업로드합니다.
2. 업로드 완료 후 서버에 완료 메시지를 전송합니다.
3. 서버는 데이터베이스에서 영상을 불러와 분석을 시작합니다.
4. 운동 자세를 분석하고 피드백을 생성합니다.
5. 분석 결과(이미지, 피드백 내용, 통계 수치 등)를 데이터베이스에 업로드합니다.
6. 분석 완료 메시지를 클라이언트에 전송합니다.
7. 클라이언트는 데이터베이스에서 분석 결과를 확인합니다.

---

## API 문서

**서버에 분석을 요청할 수 있는 API입니다.**

| HTTP Method | POST |
|-------------|------|
| 요청 URL | http://[serverIP]:[PORT]/download_and_analyze |
| 요청 파라미터 | ?data=[업로드된_영상_경로] |
| 응답 | 분석 결과 문자열 또는 오류 코드 (505, 504 등) |

- **[serverIP] :** 서버에 접속 가능한 외부 IP 주소
- **[PORT] :** 접속 가능한 포트 번호 (기본 5000 사용)
- **[path_your_video] :** 사용자가 업로드한 영상 경로

**예시:**  
`http://12.345.678.90:5000/download_and_analyze?data=temp/video/user/drj9802@gmail.com/exercise_Squat_2305092259`

> 분석 결과 문자열(RESULT)을 수신하면, 분석 결과가 DB 내 사용자의 폴더에 저장된 상태입니다.

---

## 설치 및 실행 방법

먼저 **Docker**를 설치해야 합니다.

🔗 [Docker Desktop 다운로드](https://www.docker.com/products/docker-desktop)

### [방법 1] DockerHub 이용

DockerHub에서 이미지를 간편하게 다운로드할 수 있습니다.

- DockerHub 링크: [DockerHub - chohyunsik/dothis](https://hub.docker.com/repository/docker/chohyunsik/dothis/general)

또는 아래 명령어를 사용해 다운로드할 수 있습니다:

```bash
docker pull chohyunsik/dothis:latest
```

💡 이 이미지는 **Linux, Windows OS**에서 사용 가능합니다.

다운로드 후, 아래 명령어로 서버를 실행할 수 있습니다:

```bash
docker container run -d -p5000:5000 chohyunsik/dothis
```

---

### [방법 2] GitHub 이용

GitHub에서 직접 소스 코드를 다운로드할 수 있습니다.

```bash
git clone https://github.com/CHOHYUNSIK/Flask-docker.git
```

다운로드 후 아래 디렉토리로 이동:

```bash
cd [다운로드된_경로]/Flask-docker/Flask-Docker/app/api
```

`docker-compose.yml` 파일을 사용하여 실행:

```bash
docker-compose up
```

---

## 문의

서버에 문제가 있거나 궁금한 사항이 있다면 **이메일로 문의**해주세요.

📧 **whgustlr0326@gmail.com**
