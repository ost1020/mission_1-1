# 코디세이 미션 1: 개발 환경 세팅 및 도구 활용

## 1. 프로젝트 개요
* **목표:** 개발을 위한 워크스테이션 구축 및 핵심 도구(터미널, Docker, Git) 활용법 습득
* **요약:** 리눅스 CLI(터미널) 조작, Docker를 이용한 격리된 컨테이너 실행 및 커스텀 이미지 제작, 포트 매핑과 볼륨을 통한 데이터 영속성 검증, 협업을 위한 Git/GitHub 환경 세팅

## 2. 실행 환경
* **OS:** macOS (Apple Silicon)
* **Shell:** zsh 
* **Terminal:** macOS 기본 터미널
* **Docker:** (여기에 `docker --version` 결과 입력. 예: Docker version 26.x.x)
* **Git:** (여기에 `git --version` 결과 입력. 예: git version 2.x.x)

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
$ pwd
/Users/macbook (본인의 실제 경로)
$ mkdir codyssey-mission
$ cd codyssey-mission
$ touch test.txt
$ ls -la
(출력 결과 복사붙여넣기)
$ cp test.txt copy.txt
$ mv copy.txt rename.txt
$ cat test.txt
$ rm rename.txt
```

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
$ docker run hello-world
(출력 결과 복사붙여넣기)

(이후 ubuntu 컨테이너 실행 관련 로그 및 관찰 내용 정리)
```

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