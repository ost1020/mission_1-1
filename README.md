# 코디세이 미션 1: 개발 환경 세팅 및 도구 활용

## 1. 프로젝트 개요
* **목표:** 개발을 위한 워크스테이션 구축 및 핵심 도구(터미널, Docker, Git) 활용법 습득
* **요약:** 리눅스 CLI(터미널) 조작, Docker를 이용한 격리된 컨테이너 실행 및 커스텀 이미지 제작, 포트 매핑과 볼륨을 통한 데이터 영속성 검증, 협업을 위한 Git/GitHub 환경 세팅

## 2. 실행 환경
* **OS:** macOS (Apple Silicon)
* **Shell:** zsh (`/bin/zsh`)
* **Terminal:** vscode
* **Docker:** Docker version 29.9.2, build a82e528
* **Git:** git version 2.56.0

## 3. 수행 체크리스트
- [x] 터미널 기본 조작 및 폴더 구성
- [x] 권한 변경 실습
- [x] Docker 설치 및 기본 점검
- [ ] 컨테이너 실행 실습 (hello-world, ubuntu)
- [ ] 기존 Dockerfile 기반 커스텀 이미지 제작
- [ ] 포트 매핑 및 접속 증거
- [ ] 바인드 마운트 반영 증거
- [ ] Docker 볼륨 영속성 검증
- [ ] Git 설정 및 GitHub 연동

---

## 4. 수행 로그 및 증거

### 4.1. 터미널 조작 로그 기록
**검증 방법:** 현재 위치 확인, 폴더/파일 생성, 복사, 이동, 삭제 명령을 수행하여 확인
```bash
# 1. 위치 확인 및 폴더 생성/진입
taek@OHs-MacBook-Air mission_1-1 % pwd
/Users/taek/mission_1-1
# pwd (Print Working Directory): 현재 내가 위치한 폴더의 전체 경로를 알려줌. 터미널 안에서 "나 지금 컴퓨터 안에서 정확히 어디쯤 있지?" 하고 길을 잃었을 때 치는 나침반 같은 명령어.
taek@OHs-MacBook-Air mission_1-1 % mkdir codyssey-practice
# mkdir (Make Directory): 새 폴더를 만드는 명령어. mkdir codyssey-practice로 실습 전용 방을 하나 새로 팜.
taek@OHs-MacBook-Air mission_1-1 % cd codyssey-practice
# cd (Change Directory): 다른 폴더로 이동할 때 씀. 방금 만든 codyssey-practice 방 안으로 쏙 들어감. (cd ..이라고 치면 다시 바깥 폴더로 빠져나올 수 있음.)

# 2. 빈 파일 생성 및 권한/상세 리스트 확인
taek@OHs-MacBook-Air codyssey-practice % touch text.txt
# touch: 내용이 텅 빈 새 파일을 뚝딱 만들어주는 명령어. touch text.txt를 쳐서 텍스트 파일을 하나 생성함.
taek@OHs-MacBook-Air codyssey-practice % ls -la
# ls -la (List): 현재 폴더 안에 뭐가 있는지 목록을 쫙 보여줌. -l 옵션은 권한, 용량, 만든 날짜 같은 상세 정보를 보겠다는 뜻, -a 옵션은 이름 앞에 점(.)이 붙은 숨겨진 파일까지 남김없이 다 보여달라는 뜻.

# 3. 파일 복사, 이동(이름 변경), 내용 출력, 삭제 실습
taek@OHs-MacBook-Air codyssey-practice % cp text.txt copy.txt
# cp (Copy): 파일 복사하기. cp text.txt copy.txt는 원본 파일(text.txt)을 복사해서 copy.txt라는 똑같은 파일을 하나 더 만듦.
taek@OHs-MacBook-Air codyssey-practice % mv copy.txt rename.txt
# mv (Move): 파일의 위치를 옮기거나, 이름을 바꿀 때 씀. 지금은 mv copy.txt rename.txt라고 쳐서 파일 이름을 rename.txt로 쓱 바꿔치기함.
taek@OHs-MacBook-Air codyssey-practice % cat text.txt
# cat (Concatenate): 파일 안에 적힌 내용을 터미널 화면에 텍스트로 그대로 출력해서 보여줌. 지금은 빈 파일이었기 때문에 아무 글자도 안 나오고 끝남.
taek@OHs-MacBook-Air codyssey-practice % rm rename.txt
# rm (Remove): 파일을 삭제하는 명령어. 윈도우나 맥의 휴지통으로 가는 게 아니라 그 자리에서 즉시 영구 삭제되니까 실전에서는 꽤 조심해서 써야 함. rm rename.txt로 필요 없어진 파일을 깔끔하게 날려버림.
```
**추가 학습 및 Q&A**

❓ 나의 질문: 실습 중 mv copy.txt rename.txt 명령어를 통해 파일의 '이름'을 바꿨는데, 파일의 '위치'를 다른 폴더로 옮길 때는 mv를 어떻게 사용해야 하는지 궁금함.
💡 AI의 답변:
파일을 다른 폴더로 옮길 때는 mv [옮길 파일명] [목적지 폴더 경로] 순서로 입력하면 된다. mv 명령어 하나로 파일의 경로(위치)와 이름을 모두 제어할 수 있다. 자주 쓰이는 3가지 활용법은 다음과 같다.
하위 폴더(새 방)로 이동: mv text.txt backup/
상위 폴더(바깥 방)로 끄집어낼 때: mv text.txt .. (..은 상위 디렉토리를 의미함)
위치를 옮기며 이름도 동시에 바꿀 때: mv text.txt backup/new.txt

❓ 나의 질문: 문서 작성 중 코드블록 안팎에서 # 기호를 쓸 때, 이게 실행에 관여하지 않는 '주석(죽은 줄)'으로 쓰이는지 아니면 단순히 글자를 강조(제목)하는 용도로 쓰이는지 헷갈림.
💡 AI의 답변:
# 기호는 쓰이는 위치(코드블록 안과 밖)에 따라 역할이 완전히 달라진다.
코드블록(예: bash ... ) 안에서: 마크다운 문법이 아닌 해당 언어(bash)의 규칙을 따른다. 따라서 컴퓨터가 실행하지 않고 건너뛰는 '주석(Comment, 죽은 줄)' 역할을 하므로 안전하게 부연 설명을 적을 수 있다.
코드블록 밖(일반 마크다운)에서: 줄 맨 앞에 쓰면 글자를 크고 진하게 만들어주는 '제목(Header, 강조)' 역할을 한다. (예: ### 4.1.)

### 4.2. 권한 실습 및 증거 기록
**검증 방법:** 파일과 디렉토리를 생성하고 `chmod` 명령어로 권한 변경 전과 후를 비교
```bash
$ mkdir test_dir
$ ls -la
(변경 전 권한 확인 출력 결과)

$ chmod 777 test.txt
$ chmod 755 test_dir
$ ls -la
(변경 후 권한 확인 출력 결과 - 어떻게 바뀌었는지 비교)
```

### 4.3. Docker 설치 및 기본 점검
**검증 방법:** 터미널에서 버전을 확인하고 도커 데몬 동작 여부를 점검
```bash
$ docker --version
(출력 결과 복사붙여넣기)

$ docker info
(출력 결과 중 일부 발췌해서 붙여넣기)
```

### 4.4. 컨테이너 실행 실습
**검증 방법:** `hello-world`와 `ubuntu` 컨테이너를 차례로 실행하고 내부 진입 실험 수행
```bash
taek@OHs-MacBook-Air mission_1-1 % docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
58dee6a49ef1: Pull complete 
c3bdf82c34d1: Download complete 
Digest: sha256:c567b231f65614688711a75f31518a064258a51d442117a3e771a3a8fdba78ef
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
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

taek@OHs-MacBook-Air mission_1-1 % docker run -it ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
6f01cc7d6fb3: Pull complete 
afa7f7d582de: Pull complete 
d597c14cfeb9: Download complete 
Digest: sha256:6a06111c3b0a8600feadb270aec771dc239bf407b25625f93f44ab00e11f4d3c
Status: Downloaded newer image for ubuntu:latest
root@753aa9e3ea74:/# ls
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var
root@753aa9e3ea74:/# echo hi
hi
root@753aa9e3ea74:/# exit
exit```

### 4.5. 기존 Dockerfile 기반 커스텀 이미지 제작
* **선택한 베이스 이미지:** (예: NGINX, Ubuntu 등)
* **커스텀 포인트 목적:** (예: 나만의 정적 웹페이지 콘텐츠 추가)
* **작성한 Dockerfile:**
```dockerfile
(작성한 Dockerfile 코드 복사붙여넣기)
```
* **빌드 및 실행 로그:**
```bash
(docker build 및 docker run 명령어와 그 결과 복사붙여넣기)
```

### 4.6. 포트 매핑 및 접속 증거
**검증 방법:** `-p` 옵션으로 포트 매핑 실행 후 브라우저 접속 화면 확인
* (브라우저 주소창에 `localhost:포트번호`를 입력하여 접속한 화면 캡처 이미지 첨부)

### 4.7. 바인드 마운트 및 볼륨 영속성 검증
**검증 방법:** 
1. **바인드 마운트:** 명령어로 호스트 폴더 연결 후 호스트 파일 변경이 컨테이너에 반영되는지 확인
2. **볼륨:** `docker volume create`로 볼륨 생성 및 연결, 컨테이너 삭제 후 재생성 시 데이터 유지 확인
```bash
(바인드 마운트 테스트 관련 명령어 및 결과 로그)

(볼륨 생성, 컨테이너 연결, 데이터 작성, 컨테이너 삭제, 재생성, 데이터 확인 관련 전체 로그 복사붙여넣기)
```

### 4.8. Git 설정 및 GitHub 연동
**검증 방법:** 사용자 정보 설정 확인 및 VSCode에서 GitHub 연동 상태 확인
```bash
taek@OHs-MacBook-Air ~ % git config --global user.name ost1020
taek@OHs-MacBook-Air ~ % git config --global user.email ost102***@gmail.com
taek@OHs-MacBook-Air ~ % git config --global --unset init.defualbrainch
taek@OHs-MacBook-Air ~ % git config --list                             
credential.helper=osxkeychain
user.name=ost1020
user.email=ost102***@gmail.com
init.defaultbranch=main
```

![깃허브 연동 증명](./비주얼 스튜디오 깃허브 연동.png)

---

## 5. 트러블슈팅

### 트러블슈팅 1: (발생한 문제 요약)
* **문제:** (어떤 에러 메시지가 났는지, 어떤 상황이었는지)
* **원인 가설:** (왜 이런 문제가 발생했다고 생각했는지)
* **확인:** (가설이 맞는지 어떤 명령어나 방법으로 확인했는지)
* **해결/대안:** (최종적으로 어떻게 해결했는지)

### 트러블슈팅 2: (발생한 문제 요약)
* **문제:** 
* **원인 가설:** 
* **확인:** 
* **해결/대안:** 