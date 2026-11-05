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
- [x] Docker 설치/점검
- [x] hello-world 실행
- [x] Dockerfile 빌드/실행
- [x] 포트 매핑 접속(2회)
- [x] 바인드 마운트 반영
- [x] 볼륨 영속성
- [x] Git 설정 + VSCode GitHub 연동

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
#### **추가 학습 및 Q&A**

> ❓ 나의 질문: 실습 중 mv copy.txt rename.txt 명령어를 통해 파일의 '이름'을 바꿨는데, 파일의 '위치'를 다른 폴더로 옮길 때는 mv를 어떻게 사용해야 하는지 궁금함.
>
> 💡 AI의 답변:
> 파일을 다른 폴더로 옮길 때는 mv [옮길 파일명] [목적지 폴더 경로] 순서로 입력하면 된다. mv 명령어 하나로 파일의 경로(위치)와 이름을 모두 제어할 수 있다. 자주 쓰이는 3가지 활용법은 다음과 같다.
> 하위 폴더(새 방)로 이동: mv text.txt backup/
> 상위 폴더(바깥 방)로 끄집어낼 때: mv text.txt .. (..은 상위 디렉토리를 의미함)
> 위치를 옮기며 이름도 동시에 바꿀 때: mv text.txt backup/new.txt
>
> ❓ 나의 질문: 문서 작성 중 코드블록 안팎에서 # 기호를 쓸 때, 이게 실행에 관여하지 않는 '주석(죽은 줄)'으로 쓰이는지 아니면 단순히 글자를 강조(제목)하는 용도로 쓰이는지 헷갈림.
>
> 💡 AI의 답변:
> '#' 기호는 쓰이는 위치(코드블록 안과 밖)에 따라 역할이 완전히 달라진다.
> 코드블록(예: bash ... ) 안에서: 마크다운 문법이 아닌 해당 언어(bash)의 규칙을 따른다. 따라서 컴퓨터가 실행하지 않고 건너뛰는 '주석(Comment, 죽은 줄)' 역할을 하므로 안전하게 부연 설명을 적을 수 있다.
> 코드블록 밖(일반 마크다운)에서: 줄 맨 앞에 쓰면 글자를 크고 진하게 만들어주는 '제목(Header, 강조)' 역할을 한다. (예: ### 4.1.)

### 4.2. 권한 실습 및 증거 기록
**검증 방법:** 파일과 디렉토리를 생성하고 `chmod` 명령어로 권한 변경 전과 후를 비교

```bash
taek@OHs-MacBook-Air codyssey-practice % cd ~/mission_1-1/codyssey-practice
# cd (Change Directory): 폴더를 이동하라는 명령어. 
# ~ (물결표, Tilde): 내 맥북 기준 '홈 디렉토리(Home Directory)'를 의미하는 특별한 기호. 맥북 기준으로는 /Users/taek이라는 절대 경로와 완전히 똑같음. 내가 컴퓨터 어디에 숨어 있든 ~ 하나만 치면 바로 개인 폴더(집)로 순간이동할 수 있음.
# / (슬래시): 폴더와 폴더 사이를 구분하는 '경로 구분자'. (윈도우의 \와 같은 역할.)
# 해석: "내 홈 폴더(~) 안에 있는(/) mission_1-1 폴더 안에 있는(/) codyssey-practice 폴더로 이동해라."
taek@OHs-MacBook-Air codyssey-practice % touch perm_test.txt
# touch: 원래는 파일의 수정 시간을 현재 시간으로 톡 건드려(touch) 업데이트하는 명령어인데, '내용이 없는 빈 파일'을 뚝딱 만들 때 가장 많이 씀.
# 해석: "아무 내용이 없는 빈 텍스트 파일(perm_test.txt)을 생성해라."
taek@OHs-MacBook-Air codyssey-practice % ls -l
total 0
-rw-r--r--@ 1 taek  staff  0 Nov  3 15:35 perm_test.txt
-rw-r--r--@ 1 taek  staff  0 Nov  3 15:05 text.txt
# ls (List): 현재 폴더 안의 파일 목록을 보여줌.
# -l (Long format): 단순한 이름이 아니라 파일의 권한(자물쇠 상태), 소유자, 용량, 만든 날짜 등 '상세 정보'를 길게(long) 보여달라는 옵션.
# 1번째 칸 (-): 파일 종류. 이게 일반 파일인지 폴더인지 알려줌. -면 일반 파일, d면 디렉토리(폴더)라는 뜻이야.
# 2~4번째 칸 (rw-): 소유자(나, User)의 권한. 이 파일을 만든 '나'는 무엇을 할 수 있나? r(Read, 읽기)과 w(Write, 쓰기)는 가능하고, 세 번째 자리가 비어있으니(-) 프로그램처럼 실행(eXecute)은 못 한다는 뜻.
# 5~7번째 칸 (r--): 그룹(Group)의 권한. 나와 같은 그룹에 속한 사람들은 무엇을 할 수 있나? r(읽기)만 가능함.
# 8~10번째 칸 (r--): 기타 사용자(Others)의 권한. 나머지 모든 사람들은 무엇을 할 수 있나? 역시 r(읽기)만 가능함.
# 결론적으로 -rw-r--r--는 "나(소유자)는 읽고 수정할 수 있지만, 남들은 구경(읽기)만 할 수 있는 평범한 텍스트 파일"이라는 뜻.
taek@OHs-MacBook-Air codyssey-practice % chmod 700 perm_test.txt
# chmod (Change Mode): 파일이나 폴더의 권한을 변경하는 명령어.
# 700 (숫자 권한 방식): 세 자리 숫자는 각각 소유자(나) / 그룹 / 기타 사용자의 권한을 뜻함.
# 첫 번째 7 (소유자): 읽기(4) + 쓰기(2) + 실행(1)의 값을 더한 수. 즉, 모든 권한(4+2+1=7)을 나에게 줌.
# 두 번째 0 (그룹): 아무 권한 없음.
# 세 번째 0 (기타): 아무 권한 없음.
# 해석: "이 파일은 나만 읽고 쓰고 실행할 수 있고, 남들은 절대 건드리지 못하게 강력한 자물쇠를 채워라."
taek@OHs-MacBook-Air codyssey-practice % ls -l
total 0
-rwx------@ 1 taek  staff  0 Nov  3 15:35 perm_test.txt
-rw-r--r--@ 1 taek  staff  0 Nov  3 15:05 text.txt
# perm_test.txt 파일의 권한이 '-rw-r--r--'에서 '-rwx------'로 변경됨.
taek@OHs-MacBook-Air codyssey-practice % echo "echo 'Hello Permission'" > run.sh
# echo: 뒤에 오는 문장을 화면에 메아리(echo) 치듯 그대로 출력하라는 뜻.
# > (리다이렉션 기호): 출력의 방향을 화면이 아니라 '파일'로 돌려버리는 기호임.
# 해석: "화면에 echo 'Hello Permission'이라는 글자를 띄우지 말고, 그 글자를 그대로 run.sh라는 파일 안에 덮어써서(저장해서) 만들어라." (한 줄의 명령어로 간단한 스크립트 파일을 만든 거임)
taek@OHs-MacBook-Air codyssey-practice % ls -l
total 8
-rwx------@ 1 taek  staff   0 Nov  3 15:35 perm_test.txt
-rw-r--r--@ 1 taek  staff  24 Nov  3 15:37 run.sh
-rw-r--r--@ 1 taek  staff   0 Nov  3 15:05 text.txt
# run.sh 파일이 새로 생성됐고, run.sh에 echo 내용을 덮어쓰면서 바이트 블록의 크기가 변함.
taek@OHs-MacBook-Air codyssey-practice % chmod +x run.sh
# +x (기호 권한 방식): 숫자가 아닌 직관적인 기호로 권한을 주는 방식. +는 추가하겠다, x는 실행(eXecute) 권한을 의미함.
# 해석: "아까 만든 run.sh는 그냥 텍스트 파일(메모장)에 불과했는데, 이제 컴퓨터가 이걸 '프로그램'으로 인식해서 실행할 수 있도록 실행 권한(+x)을 달아줘라."
taek@OHs-MacBook-Air codyssey-practice % ls -l
total 8
-rwx------@ 1 taek  staff   0 Nov  3 15:35 perm_test.txt
-rwxr-xr-x@ 1 taek  staff  24 Nov  3 15:37 run.sh
-rw-r--r--@ 1 taek  staff   0 Nov  3 15:05 text.txt
taek@OHs-MacBook-Air codyssey-practice % ./run.sh
Hello Permission
# . (점): 리눅스에서 점 하나는 '현재 내가 있는 폴더(현재 위치)'를 뜻함.
# / (슬래시): 폴더 구분자.
# 해석: "현재 폴더(.) 안에 있는(/) run.sh를 실행해라."
# 왜 굳이 ./를 붙일까?: 리눅스/맥은 보안이 철저해서, 현재 폴더에 있는 프로그램이라도 이름만 치면(예: run.sh) 해킹 파일일지 모르니 튕겨내 버림. 그래서 반드시 "지금 내 위치에 있는 이 파일!"이라고 정확한 경로(./)를 콕 집어서 허락해 줘야만 실행됨.
taek@OHs-MacBook-Air codyssey-practice % mkdir test_dir
# test_dir이라는 새로운 폴더 생성
taek@OHs-MacBook-Air codyssey-practice % ls -l
total 8
-rwx------@ 1 taek  staff   0 Nov  3 15:35 perm_test.txt
-rwxr-xr-x@ 1 taek  staff  24 Nov  3 15:37 run.sh
drwxr-xr-x@ 2 taek  staff  64 Nov  3 17:02 test_dir
-rw-r--r--@ 1 taek  staff   0 Nov  3 15:05 text.txt
# 폴더 내 파일 목록과 각 파일의 상세 정보를 출력
taek@OHs-MacBook-Air codyssey-practice % chmod 777 test_dir
# test_dir에 대해 사용자, 그룹, 기타 사용자 모두에게 읽고(4), 쓰고(2), 실행(1)할 수 있는 권한을 부여
taek@OHs-MacBook-Air codyssey-practice % ls -l
total 8
-rwx------@ 1 taek  staff   0 Nov  3 15:35 perm_test.txt
-rwxr-xr-x@ 1 taek  staff  24 Nov  3 15:37 run.sh
drwxrwxrwx@ 2 taek  staff  64 Nov  3 17:02 test_dir
-rw-r--r--@ 1 taek  staff   0 Nov  3 15:05 text.txt
# drwxr-xr-x에서 drwxrwxrwx으로 변경
```

#### 권한 변경 전후 비교
* **`perm_test.txt` (일반 파일) 권한 축소:**
  * **변경 전 (`ls -l`):** `-rw-r--r--` (소유자는 읽고 쓰기 가능, 남들은 읽기만 가능)
  * **변경 후 (`chmod 700`):** `-rwx------` (소유자에게만 모든 권한을 부여하고 남들은 접근 차단)
* **`run.sh` (스크립트 파일) 실행 권한 부여:**
  * **변경 전 (`ls -l`):** `-rw-r--r--` (실행 불가능한 일반 텍스트 문서)
  * **변경 후 (`chmod +x`):** `-rwxr-xr-x` (실행 권한 `x`가 추가되어 `./run.sh`로 실행 가능)
* **`test_dir` (디렉토리) 모든 권한 개방:**
  * **변경 전 (`ls -l`):** `drwxr-xr-x` (소유자 외에는 읽기 및 진입만 가능)
  * **변경 후 (`chmod 777`):** `drwxrwxrwx` (모든 사용자에게 디렉토리 내부 쓰기 권한까지 전면 개방)

#### **추가 학습 및 Q&A**

> **❓ 나의 질문:** `touch`는 원래 파일의 수정 시간을 현재로 업데이트하는 명령어라는데, 파일 내용은 안 바꾸고 '시간'만 업데이트하는 게 무슨 의미가 있으며, 이것이 어떻게 '빈 파일 만들기'로 쓰이게 된 것인지 궁금함.
>
> **💡 AI의 답변:** 
> **1. '수정 시간'을 강제로 업데이트하는 이유**
> 시스템 자동 백업이나 코드 빌드(컴파일) 시, 컴퓨터는 "가장 최근에 수정 시간이 변경된 파일"만 골라서 다음 작업을 수행한다. 이때 파일 내용은 그대로 두더라도 시스템에 "나 방금 작업했으니 백업(또는 빌드) 목록에 당장 포함시켜!"라고 억지로 신호를 주기 위해 `touch`로 시간만 조작(업데이트)하는 기능이 필요하다.
>
> **2. 어떻게 '빈 파일 생성기'로 연결되었나?**
> 리눅스 설계자들은 `touch` 명령어에 아주 자연스러운 규칙을 하나 추가했다.
> *"시간을 업데이트하라고 지시받은 파일이 만약 현재 폴더에 없다면? 에러를 띄우지 말고, 일단 빈 파일을 새로 만든 다음 그 파일의 시간을 '지금'으로 찍어주자!"*
> 텍스트 편집기를 열고 닫을 필요 없이 `touch 파일명` 명령어 한 줄만 치면 0바이트짜리 빈 껍데기 파일을 순식간에 만들 수 있기 때문에, 이 엄청난 편리함 덕분에 현재는 전 세계 개발자들에게 '초고속 빈 파일 생성기'로 굳어져서 쓰이고 있다.
>
> **❓ 나의 질문:** `codyssey-practice`는 분명히 폴더인데, 왜 목록에서 `d`가 아니라 `-`로 표시되는지?
>
> **💡 AI의 답변:** 
> 터미널 상의 현재 위치가 이미 `codyssey-practice` 폴더 '내부'로 들어와 있기 때문이다. 방 안에 들어와서 바닥에 놓인 '일반 파일'들만 조회했으므로 첫 글자가 `-`로 출력된 것이다. 
> 해당 폴더 자체의 자물쇠 상태(`d`)를 보려면 상위 폴더로 한 칸 나가서(`cd ..`) 조회하거나, 현재 위치한 디렉토리 자체의 정보를 보여주는 명령어(`ls -ld .`)를 사용해야 한다.
>
> **❓ 나의 질문:** 
> 1) 내가 만든 파일인데 왜 소유자인 나한테도 '실행' 권한이 없는지? 
> 2) `ls -l` 결과 맨 위에 뜨는 `total 0`은 무엇인지? 
> 3) 소유자/그룹/기타 사용자를 나누는 기준은 어떤 환경을 가정한 것인지?
>
> **💡 AI의 답변:** 
> 1. **기본 실행 권한이 없는 이유 (보안):** 악성 코드나 알 수 없는 파일이 의도치 않게 실행되는 것을 막기 위해, 시스템은 파일 생성 시 기본적으로 실행 권한을 주지 않는다. 실행이 필요할 때만 사용자가 직접 부여(`chmod +x`)해야 한다.
> 2. **`total 0`의 의미:** 디렉토리 내 파일들이 차지하는 디스크 블록의 총합이다. 현재 파일들은 `touch`로 생성된 0바이트의 빈 파일이므로 차지하는 용량이 없어 0으로 표시된다.
> 3. **다중 사용자 환경의 권한 구분:** 리눅스/유닉스는 여러 사람이 동시에 접속하는 '서버' 환경을 전제로 설계되었다.
>    - **소유자(User):** 파일을 생성한 주체 (`taek`).
>    - **그룹(Group):** 특정 권한을 공유하는 사용자 묶음 (맥의 경우 기본 `staff` 그룹에 속함).
>    - **기타 사용자(Others):** 동일 시스템에 로그인할 수 있는 다른 모든 계정 (예: 게스트 계정 등).
>
> **❓ 나의 질문:** 지금 내 맥북은 혼자 쓰는 로컬 환경인데도, 시스템 자체가 '서버(다중 사용자)' 환경을 전제로 설계되었다는 뜻인지?
>
> **💡 AI의 답변:** 
> 정확하다. macOS와 Linux의 뿌리인 '유닉스(Unix)'는 거대한 컴퓨터 한 대에 여러 명이 동시 접속해 사용하는 환경을 위해 탄생했다.
> 비록 현재는 개인용 노트북으로 사용하고 있지만, 이 '다중 사용자 기반의 엄격한 권한 시스템'은 그대로 유지되고 있다. 오늘날 이 시스템은 내가 설치한 특정 앱이나 다운로드한 파일(악성 코드 등)이 시스템 전체를 망가뜨리거나 다른 프로그램의 데이터를 훔쳐보지 못하도록 격리하는 핵심 보안 방어막 역할을 한다.
>
> **❓ 나의 질문:** 권한을 표시할 때 읽기=4, 쓰기=2, 실행=1(또는 e?)로 표기하는 것이 맞는지? 또 다른 숫자도 존재하는지 궁금함.
>
> **💡 AI의 답변:** 
> 실행은 `e`가 아닌 **`x`**(eXecute)로 표기한다. 권한을 나타내는 기본 숫자는 **읽기(r)=4, 쓰기(w)=2, 실행(x)=1, 권한 없음(-)=0**이 맞다. 
> 
> 새로운 숫자가 존재하는 것은 아니며, 이 숫자들을 더해서 **0부터 7까지의 조합**을 만들어 사용한다. 
> (예: 7 = 4+2+1 모든 권한 / 6 = 4+2 읽기+쓰기 / 5 = 4+1 읽기+실행)
>
> 이 숫자들이 1, 2, 4인 이유는 이진수(비트) 스위치 방식을 따르기 때문이다. 이 방식을 사용하면 권한의 숫자 합(예: 3은 무조건 2+1)이 절대 중복되지 않아 컴퓨터가 권한 상태를 정확하게 해석할 수 있다.
>
> **❓ 나의 질문:** `echo` 명령어와 리다이렉션(`>`) 기호를 쓸 때, 지정한 파일(`run.sh`)이 없으면 `touch`처럼 알아서 새 파일을 만들고 내용을 덮어쓰는 방식으로 작동하는 것인지 궁금함.
>
> **💡 AI의 답변:** 
> 정확하게 유추했다. 리다이렉션(`>`) 기호는 지정한 파일이 현재 디렉토리에 없으면 에러를 띄우는 대신 **자동으로 새로운 파일을 생성한 뒤 그 안에 출력값을 저장**한다. 
> 
> *참고: 리다이렉션 기호의 종류*
> * **`>` (덮어쓰기):** 파일이 없으면 새로 만들고, 이미 있으면 **기존 내용을 모두 지우고** 새 내용으로 덮어쓴다.
> * **`>>` (이어쓰기):** 파일이 없으면 새로 만들고, 이미 있으면 기존 내용을 보존한 채 **맨 아랫줄에 내용을 추가(Append)**한다.
>
> **❓ 나의 질문:** 리다이렉션 기호(`>`)는 `echo` 명령어에만 쓰이는 것인지, 아니면 다른 명령어와도 조합해서 쓸 수 있는지 궁금함.
>
> **💡 AI의 답변:** 
> 리다이렉션은 특정 명령어에 종속된 기능이 아니라 '쉘(Shell)' 자체의 기능이므로, 화면에 결과(Standard Output)를 출력하는 **모든 명령어와 결합하여 사용할 수 있다.**
> 
> *활용 예시:*
> * `ls -l > list.txt` : 현재 디렉토리의 파일 상세 목록을 텍스트 파일로 저장.
> * `cat file1.txt file2.txt > merge.txt` : 두 파일의 내용을 합쳐서 새로운 파일로 병합.
> * `ps aux > process_list.txt` : 현재 실행 중인 프로세스 전체 목록을 파일로 기록.
>
> **❓ 나의 질문:** `chmod +x run.sh` 명령어에서 `+x`는 소유자, 그룹, 기타 사용자 구분 없이 일괄적으로 실행 권한을 부여하는 것인지 궁금함.
>
> **💡 AI의 답변:** 
> 정확하다. 기호 방식을 사용할 때 권한을 받을 '대상'을 명시하지 않으면 기본적으로 **모든 사용자(a, All)**에게 일괄 적용된다. 즉, `+x`는 `a+x`와 완전히 동일하게 작동한다.
> (실제 실습 결과에서도 `-rw-r--r--`가 `-rwxr-xr-x`로 변경되며 세 구간 모두에 `x`가 추가된 것을 확인할 수 있다.)
> 
> *참고: 특정 대상에게만 권한을 주고 뺏는 방법*
> * `u+x` : 소유자(User)에게만 실행 권한 추가
> * `g-w` : 그룹(Group)의 쓰기 권한 뺏기
> * `o+r` : 기타 사용자(Others)에게 읽기 권한 추가
>
> **❓ 나의 질문:** `run.sh` 파일에 넣은 `echo 'Hello Permission'`이 출력될 때, `echo`가 자동으로 명령어로 기능하게 된 것인지? 그리고 명령어 작성 시 큰따옴표(`"`)와 작은따옴표(`'`)는 각각 어떤 역할을 했는지 궁금함.
>
> **💡 AI의 답변:** 
> **1. `echo`가 명령어로 작동한 이유 (쉘 스크립트의 원리):**
> 텍스트 파일(`run.sh`) 안에 단순한 글자로 저장되어 있던 `echo 'Hello Permission'`이, 파일에 **실행 권한(`+x`)을 부여하고 실행(`./run.sh`)하는 순간** 시스템에 의해 진짜 '명령어'로 해석되어 작동한 것이다. 이렇게 명령어들을 파일에 기록해 두고 실행하는 방식을 '쉘 스크립트'라고 한다.
>
> **2. 큰따옴표(`"`)와 작은따옴표(`'`)의 역할 (중첩 방지):**
> 따옴표는 띄어쓰기가 포함된 문장을 '하나의 덩어리'로 묶어주는 역할을 한다. 두 가지를 섞어 쓴 이유는 컴퓨터가 따옴표의 시작과 끝을 헷갈리지 않게 **안과 밖을 구분하기 위해서**다.
> * **바깥쪽 큰따옴표(`"..."`):** 첫 번째(바깥쪽) `echo`에게 "이 안의 내용을 쪼개지 말고 통째로 `run.sh`에 넣어라"고 지시하는 포장지 역할.
> * **안쪽 작은따옴표(`'...'`):** 파일 안에 기호 그대로 저장된 뒤, 훗날 스크립트가 실행될 때 두 번째(안쪽) `echo`가 출력할 텍스트(`Hello Permission`)를 묶어주는 역할.
>
> **❓ 나의 질문:** 쉘 스크립트(Shell Script)라는 게 정확히 무엇인지?
>
> **💡 AI의 답변:** 
> 쉘 스크립트는 여러 개의 터미널 명령어들을 텍스트 파일 하나에 순서대로 기록해 둔 **'명령어 자동화 대본(매크로)'**이다.
> 매번 수동으로 하나씩 입력해야 하는 명령어들(예: 폴더 생성, 파일 생성, 권한 변경 등 연속된 작업)을 파일 하나에 모아두고 실행 권한(`+x`)을 주면, 사용자가 파일을 실행(`./파일명.sh`)하는 순간 컴퓨터가 파일 안의 명령어들을 위에서부터 순서대로 일괄 자동 실행한다. 서버 세팅이나 자동 백업 등 반복적인 작업을 처리할 때 필수적으로 사용된다.
>
> **❓ 나의 질문:** 파일의 x는 프로그램을 실행하는 권한이라 배웠는데, 폴더(디렉토리)에 부여하는 실행(x) 권한은 어떤 역할을 하는 것인지?
>
> **💡 AI의 답변:**
> 디렉토리에서 x(eXecute) 권한은 '실행'이 아니라 '접근(진입)' 권한을 뜻한다. 즉, cd 명령어를 통해 그 폴더 안으로 들어갈 수 있는 통행증 역할이다.
> r (읽기) 권한만 있고 x (진입) 권한이 없으면: 밖에서 폴더 안의 파일 목록(ls)은 볼 수 있지만, 그 폴더 안으로 이동(cd)할 수는 없다.
> 이 때문에 디렉토리를 정상적으로 사용하려면 보통 읽기(r)와 진입(x) 권한을 세트(r-x, 숫자 5)로 묶어서 부여한다.

### 4.3. Docker 설치 및 기본 점검
**검증 방법:** 터미널에서 Docker 버전을 확인하고, 데몬(엔진)이 정상적으로 백그라운드에서 동작하고 있는지 상세 정보를 출력하여 점검한다.

```bash
taek@OHs-MacBook-Air codyssey-practice % docker --version
Docker version 29.9.2, build a82e528
taek@OHs-MacBook-Air codyssey-practice % docker info
Client:
 Version:    29.9.2
 Context:    desktop-linux
 Debug Mode: false
 # ... (중략: 플러그인 목록 정상 출력됨) ...
Server:
 Containers: 2
  Running: 0
  Paused: 0
  Stopped: 2
 Images: 2
 Server Version: 29.9.2
 Storage Driver: overlayfs
 # ... (중략: 네트워크 및 시스템 자원 정보 정상 출력됨) ...
 Operating System: Docker Desktop
 OSType: linux
 Architecture: aarch64
 CPUs: 10
 Total Memory: 7.748GiB
 Name: docker-desktop
```
#### 추가 학습 및 Q&A (명령어 옵션 -와 --의 차이)

> **❓ 나의 질문:** docker --version을 입력할 때 하이픈을 두 개(--) 붙이는 것은 어떤 의미인지 궁금함.

> **💡 AI의 답변:**
>명령어의 옵션을 부여할 때 '단축형(Short)'과 '상세형(Long)'을 구분하기 위한 표준 규칙이다.
> '-' (하이픈 1개): 알파벳 한 글자로 된 단축 옵션에 사용한다. (예: -v, -a). 여러 개를 이어서 쓸 수 있다 (예: -al).
> -- (하이픈 2개): 단어 형태로 된 상세 옵션에 사용한다. (예: --version, --help).
> 만약 -version이라고 입력하면 시스템은 이를 v, e, r 등 개별 단축 옵션 7개가 결합된 것으로 오해하므로, "이것은 하나의 단어다"라는 것을 명확히 시스템(쉘)에 전달하기 위해 --를 사용한다.

> **❓ 나의 질문:** Docker 기본 점검 단계에서 언급된 '데몬(Daemon)'이란 정확히 무엇인지 궁금함.
>
> **💡 AI의 답변:** 
> '데몬(Daemon)'은 사용자가 직접 제어하지 않아도 컴퓨터 백그라운드에서 항상 실행되며 요청을 기다리고 처리해 주는 서비스(프로그램)를 의미한다. 
> 
> 도커는 클라이언트-서버 구조로 이루어져 있다.
> * **Client (터미널의 `docker` 명령어):** 사용자의 명령(주문)을 받아 전달하는 역할.
> * **Server (도커 데몬, `dockerd`):** 클라이언트의 명령을 받아 실제로 이미지를 다운로드하고 컨테이너를 생성/실행하는 핵심 백그라운드 프로세스.
> 
> 즉, `docker info`에서 데몬 동작 여부를 확인한다는 것은 "내 명령어를 받아 실제로 처리해 줄 백그라운드 엔진이 정상적으로 켜져 있는가?"를 점검하는 필수 과정이다.

### 4.4. Docker 기본 운영 명령 수행
**검증 방법:** Nginx 이미지를 다운로드하고 컨테이너를 백그라운드에서 실행한 뒤, 상태/로그/리소스를 모니터링하고 최종적으로 종료하는 전체 과정을 점검한다.
```bash
taek@OHs-MacBook-Air codyssey-practice % docker pull nginx:alpine
alpine: Pulling from library/nginx
Status: Image is up to date for nginx:alpine
docker.io/library/nginx:alpine

taek@OHs-MacBook-Air codyssey-practice % docker images
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       22.6kB         10.3kB    U   
nginx:alpine         785835ea65db       94.1MB         27.2MB        
ubuntu:latest        6a06111c3b0a        181MB         44.9MB    U   

taek@OHs-MacBook-Air codyssey-practice % docker run -d --name my_web nginx:alpine
470eb98f3e9273ae2b4130dd0ee51015e1ec1de0264ac796238ed1215641c6dd

taek@OHs-MacBook-Air codyssey-practice % docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
470eb98f3e92   nginx:alpine   "/docker-entrypoint.…"   3 seconds ago   Up 3 seconds   80/tcp    my_web

taek@OHs-MacBook-Air codyssey-practice % docker logs my_web
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
# ... (중략) ...
2026/11/04 07:07:24 [notice] 1#1: start worker processes
2026/11/04 07:07:24 [notice] 1#1: start worker process 39

taek@OHs-MacBook-Air codyssey-practice % docker stats --no-stream my_web
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O        BLOCK I/O         PIDS
470eb98f3e92   my_web    0.00%     19.48MiB / 7.748GiB   0.25%     1.7kB / 126B   10.4MB / 12.3kB   11

taek@OHs-MacBook-Air codyssey-practice % docker stop my_web
my_web

taek@OHs-MacBook-Air codyssey-practice % docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS                     PORTS     NAMES
470eb98f3e92   nginx:alpine   "/docker-entrypoint.…"   About a minute ago   Exited (0) 4 seconds ago             my_web
753aa9e3ea74   ubuntu         "bash"                   25 hours ago         Exited (0) 25 hours ago              suspicious_ritchie
8679578d7961   hello-world    "/hello"                 25 hours ago         Exited (0) 25 hours ago              pensive_gagarin
```

**추가 학습 및 Q&A**

> **❓ 나의 질문:** `docker pull nginx:alpine` 명령어에서 `pull`은 무엇이고, `nginx:alpine`은 구체적으로 어떤 의미를 담고 있는지?
>
> **💡 AI의 답변:** 
> 스마트폰 앱스토어에서 앱을 검색하고 다운로드하는 과정과 동일한 원리다.
> * **`pull` (가져오기):** 도커 허브(Docker Hub)라는 공식 저장소에서 원하는 프로그램의 '이미지(설치 원본 파일)'를 내 컴퓨터로 다운로드하라는 명령어다.
> * **`nginx:alpine` (프로그램 이름:버전):** 다운로드할 타겟의 정확한 명칭이다.
>   * **`nginx`:** 전 세계적으로 널리 쓰이는 가볍고 빠른 웹 서버 프로그램.
>   * **`:` (콜론):** 프로그램의 이름과 버전(태그)을 구분하는 기호.
>   * **`alpine`:** 초경량 운영체제인 Alpine Linux를 기반으로 불필요한 도구를 모두 제거한 '초소형 버전'을 뜻한다. 용량이 20~30MB 수준으로 매우 작아 다운로드와 실행 속도가 매우 빠르다.
>
> **❓ 나의 질문:** `docker pull` 결과에 나온 `Status: Image is up to date...`와 마지막 줄의 `docker.io/library/nginx:alpine`은 무슨 의미인지? 마지막 줄은 docker.io라는 웹의 library 디렉토리에서 가져왔다는 뜻인지?
>
> **💡 AI의 답변:** 
> 유추한 내용이 정확히 맞다.
> * **`Status: Image is up to date...`**: 이미 로컬(내 컴퓨터)에 최신 버전의 이미지가 다운로드되어 있어서 새로 받을 필요가 없다는 의미다.
> * **`docker.io/library/nginx:alpine`**: 이미지가 실제로 저장되어 있던 '전체 주소(풀 네임)'를 뜻한다.
>   * `docker.io`: 기본 도커 레지스트리(Docker Hub 웹사이트)
>   * `library`: 도커 공식 이미지들이 모여있는 기본 네임스페이스(디렉토리)
>   * `nginx:alpine`: 다운로드한 이미지 이름과 버전(태그)
>
> **❓ 나의 질문:** `docker images` 결과에서 `ID`와 `EXTRA` 항목(특히 `U` 표시)은 무엇을 의미하는지?
>
> **💡 AI의 답변:** 
> * **`ID` (Image ID):** 다운로드된 이미지를 식별하는 고유 해시값(주민번호)이다. 긴 암호 문자열의 앞 12자리만 잘라서 보여주며, 이미지 이름 대신 이 ID를 사용해 실행이나 삭제 명령을 내릴 수 있다.
> * **`EXTRA` (U 표시):** 우측 상단 범례(`U In Use`)에서 알 수 있듯, 현재 해당 이미지를 **사용 중인 컨테이너가 존재함(In Use)**을 의미한다. (컨테이너가 실행 중이거나 종료된 채 남아있으면 표시된다.) 
>   * *참고:* 실습 시 `docker images`를 입력한 타이밍이 `nginx` 컨테이너를 생성하기 전이었기 때문에 `nginx`는 빈칸이었고, 기존에 생성해 둔 종료된 컨테이너가 남아있던 `hello-world`와 `ubuntu`에만 `U`가 표시된 것이다.

> **❓ 나의 질문:** "이미지를 사용 중인 컨테이너"라는 개념이 헷갈림. 컨테이너가 도커 내부의 웹 서버이고 그 안에서 이미지가 실행되는 것인지? 이미지와 컨테이너의 정확한 개념 차이가 궁금함.
>
> **💡 AI의 답변:** 
> "컨테이너 자체가 특정 웹 서버"인 것이 아니라, 실행한 '이미지'의 종류에 따라 컨테이너의 역할이 결정된다. 두 가지를 **붕어빵 틀(이미지)**과 **붕어빵(컨테이너)**으로 비유할 수 있다.
> 
> * **이미지 (Image - 붕어빵 틀):** 특정 프로그램(예: Nginx 웹 서버, DB 등)을 실행하는 데 필요한 모든 파일과 설정을 모아둔 '원본 설계도'이자 '설치 파일'이다. (정적인 상태)
> * **컨테이너 (Container - 붕어빵):** 원본 이미지를 바탕으로 메모리에 올라가 **실제로 작동하고 있는 격리된 프로그램**이다. (동적인 상태)
> 
> 즉, "이미지를 사용 중인 컨테이너가 존재한다(U)"는 말은, 특정 이미지(원본)를 뼈대 삼아 만들어진 컨테이너(프로그램)가 시스템에 지워지지 않고 남아있어서, 뼈대가 되는 원본 이미지를 함부로 삭제할 수 없게 잠겨있다는 뜻이다.

> **❓ 나의 질문:** `docker run -d --name my_web nginx:alpine` 명령어의 각 옵션은 무슨 역할을 하며, 실행 후 출력된 긴 문자열은 무엇인지?
>
> **💡 AI의 답변:** 
> 이미지를 기반으로 실제 컨테이너를 생성하고 실행(`run`)하는 명령어다.
> * **`-d` (Detached):** 컨테이너를 백그라운드에서 실행하여 터미널 제어권을 바로 사용자에게 돌려준다.
> * **`--name my_web`:** 생성되는 컨테이너에 랜덤 이름 대신 사용자가 지정한 이름(`my_web`)을 부여한다.
> * **`nginx:alpine`:** 컨테이너 생성의 뼈대가 되는 원본 이미지.
> 
> **실행 결과로 출력된 긴 문자열:**
> 컨테이너가 성공적으로 생성되었음을 알리는 **전체 컨테이너 ID(Full Container ID)**다. `docker ps` 목록에서는 가독성을 위해 이 긴 해시값의 앞 12자리만 잘라서 보여준다.

> **❓ 나의 질문:** 이미지를 다운받고 `run`으로 실행할 때, 어떤 이미지를 실행할지 따로 입력하지 않아도 되는 것인지?
>
> **💡 AI의 답변:** 
> 아니다. 실행 시 어떤 이미지를 뼈대로 삼을지 반드시 명시해야 하며, 실제 명령어(`docker run -d --name my_web nginx:alpine`)의 가장 마지막에 적힌 **`nginx:alpine`**이 바로 실행할 타겟 이미지를 지정한 부분이다.
> 
> *참고:* `docker run` 명령어는 매우 스마트하게 설계되어 있어서, 타겟으로 지정한 이미지가 로컬(내 컴퓨터)에 없을 경우 도커 허브에서 알아서 `pull` 작업을 먼저 수행한 뒤에 컨테이너를 실행해 준다.

> **❓ 나의 질문:** `docker ps` 명령어는 어떤 역할을 하며, 출력된 표에서 `COMMAND`, `STATUS`, `PORTS`는 각각 무엇을 의미하는지?
>
> **💡 AI의 답변:** 
> * **`docker ps` (Process Status):** 현재 '실행 중(Up)'인 컨테이너들의 목록과 상태를 보여주는 명령어다.
> * **`COMMAND`:** 컨테이너가 실행될 때 내부에서 가장 먼저 구동되도록 설정된 기본 명령어(또는 스크립트)다. (예: 웹 서버를 구동하기 위한 초기화 스크립트)
> * **`STATUS`:** 컨테이너의 현재 동작 상태. `Up`은 정상 실행 중임을, 그 뒤의 시간은 켜진 지 얼마나 되었는지를 나타낸다. (종료 시 `Exited`로 표시됨)
> * **`PORTS`:** 컨테이너가 외부 통신을 위해 개방해 둔 네트워크 포트 번호(출입문)다. `80/tcp`는 일반적인 웹(HTTP) 통신을 위해 80번 포트를 사용하고 있음을 의미한다.

> **❓ 나의 질문:** 로그(Log)라는 것의 정확한 개념은 무엇이며, Nginx 컨테이너를 실행했을 때 출력된 로그의 각 줄은 어떤 작업을 의미하는지?
>
> **💡 AI의 답변:** 
> **1. 로그(Log)의 개념:** 
> 프로그램이 실행되면서 발생하는 모든 이벤트, 에러, 상태 변화 등을 시간대별로 기록해 둔 '블랙박스(일기장)'다. 백그라운드(`-d`)로 실행된 컨테이너는 화면에 상태가 표시되지 않으므로, `docker logs`를 통해 정상 작동 여부를 확인해야 한다.
>
> **2. Nginx 실습 로그 분석:**
> * **초기 설정 단계 (`/docker-entrypoint.sh`):** 실제 웹 서버가 켜지기 전, 이미지에 내장된 초기화 스크립트가 돌아가는 과정. 네트워크(IPv6) 설정, 환경 변수 세팅 등을 수행하고 `Configuration complete`로 준비를 마친다.
> * **본격 가동 단계 (`[notice]`):** Nginx 서버가 실행되며 남긴 기록. 버전 정보(1.31.8)와 OS(Alpine)를 출력한다.
> * **워커 프로세스 생성 (`start worker process`):** 트래픽 처리를 담당할 '분신(Worker)'들을 생성하는 과정. (실습 환경인 맥북의 CPU 코어가 10개였기 때문에, Nginx가 이를 자동으로 감지하여 10개의 워커 프로세스를 생성한 것을 확인할 수 있다.)

> **❓ 나의 질문:** `stats` 명령어의 의미와 `--no-stream` 옵션의 구조, 그리고 출력된 자원 모니터링 표의 항목들은 어떤 의미인지?
>
> **💡 AI의 답변:** 
> * **`stats` (Statistics):** 컨테이너의 실시간 자원(CPU, 메모리 등) 사용량을 확인하는 모니터링 명령어다. (작업 관리자 역할)
> * **`--no-stream` 옵션의 구조:** `--`는 상세형 옵션을 뜻하며, `no-stream`은 띄어쓰기 대신 하이픈을 사용해 만든 하나의 완성된 단어다. 즉, '실시간 스트리밍(갱신)을 하지 말고 1회만 출력하라'는 뜻이다.
>
> **출력 항목(표) 분석:**
> * **CPU % / MEM USAGE:** 현재 컨테이너가 점유하고 있는 CPU와 메모리(RAM) 사용량
> * **NET I/O, BLOCK I/O:** 네트워크 통신량 및 디스크(SSD/HDD) 읽기/쓰기 발생량
> * **PIDS:** 컨테이너 내부에서 동작 중인 프로세스의 수. (이전 로그에서 확인한 Nginx 워커 프로세스 10개 + 메인 프로세스 1개 = 총 11개가 정확히 일치함)

> **❓ 나의 질문:** `docker ps -a` 결과에서 `Exited (0)`은 무엇을 의미하는지? 그리고 `PORTS`와 `NAMES` 부분은 아까 열려있던 80번 포트와 어떻게 다른 것인지?
>
> **💡 AI의 답변:** 
> * **`Exited (0)`:** 컨테이너가 아무런 에러 없이 '정상적으로 종료(Normal Exit)' 되었음을 의미한다. (0은 정상 종료 코드를 뜻함)
> * **`PORTS`와 `NAMES` 컬럼의 분리:** 
>   표에서 `PORTS`(네트워크 출입문)와 `NAMES`(컨테이너 이름)는 서로 다른 항목이다. 컨테이너가 실행 중(`Up`)일 때는 `PORTS` 항목에 `80/tcp`처럼 개방된 포트가 표시되지만, `stop` 명령으로 종료한 후에는 통신 문이 닫히기 때문에 `PORTS` 항목이 빈칸으로 표시된다. (우측 끝의 `my_web`은 `NAMES` 항목임)

> **❓ 나의 질문:** Nginx 컨테이너를 왜 굳이 백그라운드(`-d`)로 실행하라고 미션이 주어졌던 걸까?
>
> **💡 AI의 답변:** 
> Nginx와 같은 **서버용 프로그램의 특성** 때문이다. 서버는 한 번 실행되면 종료되지 않고 24시간 대기하며 트래픽을 처리해야 한다.
> * **포그라운드(옵션 없음) 실행 시:** 컨테이너가 터미널의 포커스(제어권)를 독점하게 되어 다른 명령어를 입력할 수 없다. 이를 빠져나오기 위해 강제 종료(`Ctrl+C`)를 누르면 서버 프로세스도 함께 죽어버린다.
> * **백그라운드(`-d`) 실행 시:** 컨테이너의 메인 프로세스(웹 서버)는 시스템 백그라운드에서 안전하게 계속 동작하도록 분리하고, 사용자는 즉시 터미널 제어권을 돌려받아 `docker ps`, `docker logs` 등 다른 작업을 방해 없이 수행할 수 있다.

### 4.5. 컨테이너 실행 실습
**검증 방법:** `hello-world` 이미지를 통해 기본 구동을 확인하고, 터미널 진입(`-it`)을 통해 내부 명령어를 수행해 본다. 이후 컨테이너를 빠져나올 때 완전히 종료되는 경우와 백그라운드에서 유지되는 경우의 차이를 관찰한다.

**실습 로그(1) (bash 이미지로 잘못 진입)**
```bash
taek@OHs-MacBook-Air codyssey-practice % docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

taek@OHs-MacBook-Air codyssey-practice % docker run -it --name my_ubuntu bash
latest: Pulling from library/bash
Status: Downloaded newer image for bash:latest
bash-5.3# ls
bin    etc    lib    mnt    proc   run    srv    tmp    var
dev    home   media  opt    root   sbin   sys    usr
bash-5.3# echo "Hello Ubuntu"
Hello Ubuntu
bash-5.3# exit
exit

taek@OHs-MacBook-Air codyssey-practice % docker ps -a
CONTAINER ID   IMAGE          STATUS                         NAMES
da2be5576f78   bash           Exited (0) 8 seconds ago       my_ubuntu

taek@OHs-MacBook-Air codyssey-practice % docker start my_ubuntu
my_ubuntu
taek@OHs-MacBook-Air codyssey-practice % docker exec -it my_ubuntu bash
bash-5.3# exit
exit
```
**실습 로그(2) (우분투 이미지)**

```bash
taek@OHs-MacBook-Air codyssey-practice % docker run -it --name my_ubuntu ubuntu bash
root@2f4d02ba4043:/# ls
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var
root@2f4d02ba4043:/# echo "Hello Ubuntu"
Hello Ubuntu
root@2f4d02ba4043:/# exit
exit

taek@OHs-MacBook-Air codyssey-practice % docker ps -a
CONTAINER ID   IMAGE          STATUS                      NAMES
2f4d02ba4043   ubuntu         Exited (0) 3 seconds ago    my_ubuntu

taek@OHs-MacBook-Air codyssey-practice % docker start my_ubuntu
my_ubuntu
taek@OHs-MacBook-Air codyssey-practice % docker exec -it my_ubuntu bash
root@2f4d02ba4043:/# exit
exit

taek@OHs-MacBook-Air codyssey-practice % docker ps
CONTAINER ID   IMAGE     STATUS          NAMES
2f4d02ba4043   ubuntu    Up 17 seconds   my_ubuntu
```

**추가 학습 및 Q&A**

> **❓ 나의 질문:** docker run -it 또는 docker exec -it 명령어에서 -it는 구체적으로 무슨 약자이며 어떤 역할을 하는지?

> **💡 AI의 답변:**
> it는 컨테이너 내부와 사용자가 키보드로 직접 대화(상호작용)하기 위해 두 가지 옵션을 합쳐 놓은 것이다.
> i (Interactive): 사용자가 터미널에 입력하는 키보드 입력(표준 입력, STDIN)을 컨테이너 쪽으로 계속 열어두어 전달하겠다는 뜻이다.
> t (TTY): 컨테이너 쪽에 가상의 터미널(Teletypewriter) 화면을 할당해 주어, 우리가 흔히 아는 쉘 프롬프트(bash#, root@...# 등)를 띄워주고 예쁘게 포맷팅된 텍스트를 볼 수 있게 해준다.
> 이 두 가지를 같이 써야만 비로소 우리가 리눅스 컴퓨터에 원격 접속한 것처럼 자연스럽게 명령어를 치고 결과를 볼 수 있다.

> **❓ 나의 질문:** 컨테이너 실행 실습에서 사용한 `hello-world`라는 이미지의 애초 목적과 정체는 무엇인지?
>
> **💡 AI의 답변:** 
> **1. 프로그래밍의 전통:** `Hello, World!`는 새로운 프로그래밍 언어나 환경을 처음 접할 때, 시스템이 정상적으로 작동하는지 확인하기 위해 가장 먼저 화면에 출력해보는 전 세계 개발자들의 전통적인 테스트 문구다.
> **2. 도커에서의 역할:** 도커 공식 `hello-world` 이미지는 사용자의 컴퓨터에 도커 엔진(데몬)이 정상적으로 설치되고 통신할 수 있는지 검증하기 위해 만들어진 초경량 테스트 이미지다.
> **3. 즉시 종료되는 이유:** 웹 서버(Nginx)처럼 백그라운드에서 지속적으로 대기해야 하는 서비스가 아니라, "화면에 환영 메시지를 출력한다"는 단일 목적만 가진 프로그램이기 때문이다. 메시지 출력이 끝나면 임무를 다하고 정상 종료(`Exited (0)`) 상태가 된다.

> **❓ 나의 질문:** 우분투(OS)와 배쉬(Bash)가 모두 도커 '이미지'인지? 그리고 Bash는 `nano` 같은 편집기인지, 왜 코드 블록에 `bash`라고 표기하는지 궁금함.
>
> **💡 AI의 답변:** 
> **1. 둘 다 도커 이미지가 맞다:** 도커 이미지는 OS 전체(Ubuntu)를 담을 수도 있고, 특정 단일 프로그램(Bash, Nginx) 실행에 목적을 둔 가벼운 형태로도 존재한다.
> **2. Bash의 정체 (Shell):** `nano`는 텍스트 편집기인 반면, `Bash(Bourne Again Shell)`는 사용자가 터미널에 입력한 명령어를 운영체제가 알아듣게 번역해 주는 **'명령어 해석기(통역사)'**다.
> **3. 코드 블록의 `bash` 표기:** 마크다운 작성 시 ````bash`로 묶는 것은, 해당 텍스트가 일반 문서가 아니라 터미널(Bash 쉘) 환경에서 실행할 '명령어'임을 명시하여 시각적으로 색상을 구분(Syntax Highlighting)하기 위함이다.

> **❓ 나의 질문:** `bash`는 단순한 통역사 프로그램일 뿐 OS(우분투 등)가 아닌데, 어떻게 그 컨테이너 안에서 `ls` 같은 OS 터미널 명령어를 치고 결과를 받을 수 있는지? 모든 컨테이너 내부에서는 이미지 종류와 무관하게 이런 작업이 가능한 것인지 궁금함.
>
> **💡 AI의 답변:** 
> **1. 도커 이미지의 레이어(Layer) 구조:** 
> `bash` 이미지는 통역사 프로그램 단 하나만 존재하는 것이 아니라, 그 실행을 뒷받침하기 위해 바닥에 **초경량 미니 OS(주로 Alpine Linux)가 베이스 이미지로 함께 깔려있다.** 따라서 사용자가 입력한 `ls`, `echo` 등의 명령어는 Bash를 거쳐 기저에 있는 미니 OS가 처리하여 결과를 반환하는 것이다.
>
> **2. 다른 컨테이너에서의 명령어 실행 여부:**
> 대부분의 도커 이미지(Nginx, DB 등)는 구동을 위해 내부에 최소한의 OS 환경과 기본 쉘(`sh` 또는 `bash`)을 포함하고 있다. 따라서 어떤 프로그램의 이미지든 `exec -it` 명령으로 쉘에 진입하기만 하면, 기저 OS가 제공하는 한도 내에서 자유롭게 명령어를 치고 내부를 제어할 수 있다.
>
>
> **❓ 나의 질문:** 컨테이너를 다룰 때 사용하는 `run`, `start`, `exec` 명령어는 각각 어떤 상황에서 쓰는 것이며, 정확한 차이는 무엇인지?
>
> **💡 AI의 답변:** 
> 세 가지는 컨테이너의 생애주기(Lifecycle)에서 완전히 다른 역할을 담당한다. 비유하자면 다음과 같다.
>
> * **`docker run` (창조 + 실행):**
>   * 붕어빵 틀(이미지)에서 **'새로운 붕어빵(컨테이너)을 처음 구워내면서 동시에 전원까지 켜는'** 명령어다. 내 컴퓨터에 없던 컨테이너를 새로 만들어낼 때 무조건 처음 한 번 쓴다.
>
> * **`docker start` (재부팅):**
>   * 이미 구워져 있지만 현재 **전원이 꺼져 있는(Exited) 기존 컨테이너의 '전원 버튼만 다시 누르는'** 명령어다. (새로운 컨테이너를 생성하는 것이 아님)
>
> * **`docker exec` (실행 중인 곳에 접속):**
>   * **이미 전원이 켜져서 잘 돌아가고 있는(Up)** 컨테이너에 '보조 출입문(새로운 프로세스)'을 열고 들어가 추가 명령을 내리는 명령어다. 메인 프로세스를 건드리지 않기 때문에, 볼일을 다 보고 `exit`로 빠져나와도 컨테이너의 전원은 꺼지지 않는다.
>
> **❓ 나의 질문:** `docker run -it --name my_ubuntu bash` 같은 명령어도 자연어처럼 정해진 어순(문법)이 있는지? 그리고 `--name`은 이름을 설정하라는 뜻인지?
>
> **💡 AI의 답변:** 
> **1. 도커 명령어의 어순:**
> 명령어는 일반적으로 `docker [동사(명령어)] [부사(옵션들)] [목적어(이미지)] [추가 실행 명령]`의 순서를 따른다.
> * 예: `docker run(동사) -it --name my_ubuntu(옵션들) ubuntu(이미지) bash(추가 명령)`
> * **주의점:** 옵션들 사이의 순서는 바뀌어도 무방하나, **실행할 타겟 이미지의 이름은 반드시 모든 옵션보다 뒤에 명시**해야 한다.
>
> **2. `--name` 옵션의 역할:**
> 사용자가 유추한 대로 컨테이너에 명시적인 고유 이름을 부여하는 옵션이다. 이 옵션을 생략하면 도커 엔진이 임의의 랜덤 이름(`형용사_위인이름` 조합)을 자동으로 부여하므로, 관리를 위해 실무에서는 대부분 `--name` 옵션을 통해 식별하기 쉬운 이름을 지정한다.

> **❓ 나의 질문:** 4-4 실습에서는 `pull`로 이미지를 먼저 다운받았지만, 4-5에서는 `pull` 없이 `run`만 쳐도 알아서 실행되었다. 애초에 `pull` 명령어는 생략해도 되는 것인지, 그렇다면 왜 따로 존재하는 것인지?
>
> **💡 AI의 답변:** 
> **1. `run` 명령어의 자동 `pull` 기능:** 
> `docker run` 명령어는 매우 스마트해서, 타겟 이미지가 로컬 환경에 없을 경우 자동으로 `pull` 과정을 먼저 수행한 뒤 컨테이너를 실행한다. (실제로 4-5 실습 로그 첫 줄에 `Unable to find image... Pulling from...` 메시지가 출력된 것을 확인할 수 있다.) 따라서 단순 실행이 목적이라면 생략이 가능하다.
>
> **2. `pull` 명령어를 따로 사용하는 이유 (실무적 관점):**
> * **원리 분리:** '이미지 다운로드'와 '컨테이너 생성'이 별개의 라이프사이클임을 명확히 인지하기 위함이다.
> * **사전 준비(Pre-fetching):** 실무에서는 용량이 큰 이미지를 실행 시점에 다운받으면 서비스 지연이 발생하므로, 사전에 `pull`만 따로 수행하여 이미지를 미리 준비해 두는 용도로 강력하게 활용된다.
>
> **❓ 나의 질문:** 과제 요구사항에 언급된 `attach`는 무엇이며, 우리가 실습한 `exec`와는 어떤 차이가 있는지?
>
> **💡 AI의 답변:** 
> 두 명령어 모두 실행 중인 컨테이너 내부에 접속할 때 쓰이지만, **빠져나올 때(`exit`) 컨테이너가 죽느냐 사느냐**의 결정적인 차이가 있다.
> * **`attach` (정문 접속 - 종료됨):** 컨테이너의 심장인 '메인 프로세스(PID 1)'에 직접 연결된다. 따라서 접속 후 `exit`를 입력하면 메인 프로세스가 종료되면서 컨테이너 전체의 전원이 꺼진다. (참고로 `run -it`로 처음 실행할 때도 이와 똑같이 메인 프로세스에 연결된 상태가 된다.)
> * **`exec` (뒷문 접속 - 유지됨):** 메인 프로세스는 건드리지 않고 '새로운 보조 프로세스(서브 쉘)'를 생성하여 우회 접속한다. 따라서 `exit`로 빠져나와도 보조 프로세스만 닫힐 뿐, 컨테이너는 계속 살아서(`Up`) 구동된다. 실무에서는 컨테이너를 실수로 끄는 대참사를 막기 위해 99% `exec`를 사용한다.

### 4.6. 기존 Dockerfile 기반 커스텀 이미지 제작
**검증 방법:** 기존 Nginx 이미지를 베이스로 삼아, 내가 직접 작성한 정적 웹페이지(HTML)를 띄우는 커스텀 이미지를 빌드하고 실행해 본다.

**1. 베이스 이미지 선택 및 커스텀 포인트**
* **선택한 베이스 이미지:** `nginx:alpine`
  * **선택 이유:** 가장 널리 쓰이는 안정적인 웹 서버(Nginx)이면서, 불필요한 기능이 빠진 초경량 버전(Alpine)이라 빠르고 효율적인 빌드가 가능하기 때문.
* **적용한 커스텀 포인트와 목적 (Dockerfile 구성):**
  * `FROM nginx:alpine`: 뼈대가 되는 가벼운 웹 서버 환경을 세팅한다.
  * `COPY index.html /usr/share/nginx/html/index.html`: Nginx가 기본으로 띄워주는 지루한 기본 환영 페이지를, 내가 직접 작성한 커스텀 HTML 파일(`index.html`)로 덮어씌워 나만의 문구가 뜨도록 개조한다.

**2. 빌드/실행 명령 및 핵심 결과**
```bash
# 1. 커스텀 이미지 빌드 성공 (현재 폴더의 Dockerfile 기준)
taek@OHs-MacBook-Air my-nginx % docker build -t taek_custom_nginx .
[+] Building 0.8s (7/7) FINISHED
 => [internal] load build definition from Dockerfile                      0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:785835ea65db...      0.5s
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                0.1s
 => exporting to image                                                    0.1s
 => => naming to docker.io/library/taek_custom_nginx:latest               0.0s

# 2. 빌드한 커스텀 이미지로 컨테이너 실행 (8080포트 개방)
taek@OHs-MacBook-Air my-nginx % docker run -d -p 8080:80 --name my_custom_web taek_custom_nginx
87b34ef2a630170c74426a1aed80ef747b348a19f552514061ebb2bb80e6baac

# 3. 내부 웹서버 응답 확인 (커스텀 내용 정상 출력 확인)
taek@OHs-MacBook-Air my-nginx % curl http://localhost:8080
<h1>Hello Codyssey! This is Taek's Custom Nginx Server!</h1>
```

**추가 학습 및 Q&A (HTML 태그, 이스케이프 문자, 리다이렉션 기호)**

> **❓ 나의 질문:** `echo "<h1>Hello Codyssey\!</h1>" > index.html` 명령어에서 `<h1>`의 정체, 역슬래시(`\`)의 정확한 명칭과 기능, 그리고 리다이렉션 기호(`>`)의 역할은 무엇인지?
>
> **💡 AI의 답변:**
> **1. `<h1>` (HTML 태그):**
> 리눅스/도커 명령어가 아니라 웹페이지(HTML)의 뼈대를 구성하는 문법이다. 'Heading 1'의 약자로, 브라우저에게 해당 텍스트를 가장 크고 굵은 제목(메인 헤드라인)으로 화면에 렌더링하라는 디자인 지시어다.
>
> **2. 역슬래시 `\` (이스케이프 문자, Escape Character):**
> 터미널에서 특수 기호가 가진 '특수한 기능'을 무력화(탈출, Escape)하고, **순수한 일반 텍스트 기호로 취급**하게 만드는 역할을 한다. Mac의 기본 터미널(zsh)에서는 느낌표(`!`)를 '명령어 히스토리 호출'로 인식하므로, 앞에 `\`를 붙여 일반 문자로 강제 인식시켜 에러를 방지했다.
>
> **3. 리다이렉션 기호 `>` (출력 방향 전환):**
> 터미널 화면에 출력될 내용(`echo`의 결과물)의 방향을 꺾어 파일 안으로 집어넣는 기호다.
> *   **`>` (덮어쓰기):** 파일이 없으면 새로 생성하고, 파일이 이미 존재하면 기존 내용을 완전히 지운 뒤 새 내용으로 덮어쓴다.
> *   *(참고) **`>>` (추가하기):*** 기존 내용을 지우지 않고 파일의 맨 아랫줄에 새로운 내용을 조용히 덧붙일(Append) 때 사용한다.

> **❓ 나의 질문:** `cat <<EOF` 문법을 사용할 때 터미널에 뜬 `heredoc`은 무엇이며, 이 과정에서 덮어쓰는 `Dockerfile`은 내가 만든 커스텀 이미지 자체를 의미하는 것인지?
>
> **💡 AI의 답변:**
> **1. HereDoc (Here Document):**
> 별도의 텍스트 편집기를 열지 않고, "터미널 화면 바로 여기(Here)에서 다중 입력 문서를 작성하겠다"는 의미를 가진 리눅스 문법(`<<`)의 명칭이다. 터미널에 `heredoc>`이라고 표시된 것은 현재 시스템이 입력되는 텍스트를 파일로 넘기기 위해 임시로 꽉 잡고 대기 중인 상태임을 알려주는 프롬프트다.
>
> **2. `Dockerfile`과 커스텀 이미지의 차이:**
> `Dockerfile`은 이미지 자체가 아니라, 이미지를 어떻게 만들어낼지 지시 사항을 적어둔 **'설계도(단순 텍스트 파일)'**다. 반면, 커스텀 이미지는 이 설계도를 바탕으로 `docker build` 명령어를 통해 실제로 구워낸 완성품(`taek_custom_nginx`)을 뜻한다.
>
> **3. `COPY` 명령어의 동작:**
> 호스트(맥북)에 있는 파일을 컨테이너로 '이동(Move)'시키는 것이 아니라, 원본은 그대로 둔 채 '복사(Copy)'하여 지정된 경로(`/usr/share/nginx/html/`)에 덮어씌우는 방식이다.

> **❓ 나의 질문:** `docker build -t taek_custom_nginx .` 명령어에서 `build`, `-t`, `taek_custom_nginx`, 그리고 맨 뒤의 점(`.`)은 각각 어떤 역할을 하는지?
>
> **💡 AI의 답변:**
> *   **`docker build`:** 도커 엔진에게 지정된 폴더의 `Dockerfile`(설계도)을 읽어 들여 새로운 이미지를 생성(빌드)하라는 핵심 명령어.
> *   **`-t` (Tag):** 새로 생성될 이미지에 사람이 읽고 관리하기 쉬운 '이름표(명찰)'를 부여하겠다는 옵션.
> *   **`taek_custom_nginx`:** `-t` 옵션을 통해 실제로 부여된 커스텀 이미지의 이름.
> *   **`.` (점, 현재 디렉토리):** 리눅스에서 마침표 한 개(`.`)는 '현재 작업 중인 폴더'를 의미한다. 도커에게 빌드에 필요한 재료(`Dockerfile`과 `index.html` 등)를 멀리서 찾지 말고 **"현재 있는 이 폴더 안에서 가져와서 써라"**라고 작업장(Build Context)의 위치를 지정해 주는 매우 중요한 기호다.

> **❓ 나의 질문:** `docker run -d -p 8080:80 --name my_custom_web taek_custom_nginx` 명령어에서 각 옵션의 정확한 역할과 `--name`, 그리고 맨 뒤에 적힌 이미지 이름의 관계는 무엇인지?
>
> **💡 AI의 답변:**
> *   **`docker run`:** 도커 프로그램 자체를 켜는 것이 아니라, 지정한 이미지(원본)를 바탕으로 '새로운 컨테이너'를 생성하고 가동하라는 명령어다.
> *   **`-d` (Detach):** 컨테이너를 백그라운드에서 실행하여 터미널 제어권을 뺏기지 않도록 하는 옵션.
> *   **`-p 8080:80` (Port Mapping):** `[호스트(맥북) 포트]:[컨테이너 내부 포트]`를 연결하는 옵션. 호스트의 8080번 포트로 들어오는 트래픽을 컨테이너 내부의 Nginx 전용 포트인 80번으로 연결(포워딩)해 준다.
> *   **`--name my_custom_web`:** 새로 생성되어 **실행될 컨테이너에 부여할 이름(명찰)**이다.
> *   **`taek_custom_nginx`:** 컨테이너를 만들 때 뼈대로 사용할 **원본 이미지**다. 명령어 문법상 반드시 가장 마지막에 위치해야 한다.

> **❓ 나의 질문:** 포트 매핑 옵션(`-p 8080:80`)에서 앞의 호스트 포트를 굳이 8080으로 설정한 특별한 이유가 있는지? 다른 숫자를 써도 무방한지?
>
> **💡 AI의 답변:**
> 꼭 8080이어야 할 기술적 이유는 없으며, 호스트(맥북)의 사용 중이지 않은 임의의 포트(예: 7777, 9999 등)를 자유롭게 지정해도 무방하다. 
> 8080을 주로 사용하는 이유는 개발자들 사이의 **관행**이다. 
> 1. 웹 서버의 기본 포트인 80번과의 직관적인 연관성을 주기 위함이다.
> 2. OS(macOS/Linux) 특성상 1~1024번 포트는 시스템 예약 포트라 일반 유저 권한으로 바인딩할 때 권한 오류나 기존 서비스(아파치 등)와의 충돌이 발생하기 쉬우므로, 이를 회피하기 위해 8080, 8000, 3000 등의 높은 번호 대역을 개발용으로 즐겨 쓴다.

> **❓ 나의 질문:** 터미널에서 웹 서버의 응답을 확인할 때 사용한 `curl` 명령어는 정확히 무엇이며, 브라우저로 접속하는 것과 어떤 차이가 있는지?
>
> **💡 AI의 답변:**
> `curl`(Client URL)은 커맨드라인 환경에서 데이터를 송수신할 수 있게 해주는 도구다. 
> * **일반 브라우저:** 서버로부터 받은 HTML 데이터를 시각적으로 렌더링(디자인)하여 화면에 출력한다.
> * **`curl`:** 서버에 요청을 보내고 돌아온 **'날 것의 원본 데이터(Raw 텍스트)'**를 화면 렌더링 없이 터미널에 그대로 출력한다.
> * **사용 목적:** GUI 브라우저를 띄울 필요 없이, 터미널 내에서 서버의 정상 작동 여부(포트 오픈 및 응답 상태)를 빠르고 직관적으로 테스트하기 위해 실무에서 매우 자주 사용한다.

### 4.7. 바인드 마운트(Bind Mount) 적용 및 호스트 데이터 변경 전/후 비교
**검증 방법:** 호스트(맥북)의 특정 폴더를 컨테이너 내부에 마운트(`-v`)하고, 컨테이너 내부에서 데이터를 조작했을 때 호스트의 원본 파일이 실시간으로 동기화(변경)되는지 확인한다.

**1. 호스트 변경 전 (초기 상태)**
```bash
taek@OHs-MacBook-Air my-nginx % mkdir ~/bind-test
taek@OHs-MacBook-Air my-nginx % cd ~/bind-test
taek@OHs-MacBook-Air bind-test % echo "This is Host(MacBook) Data." > memo.txt
taek@OHs-MacBook-Air bind-test % cat memo.txt
This is Host(MacBook) Data.

**2. 바인드 마운트 실행 및 컨테이너 내부에서 데이터 조작**
# 호스트의 현재 폴더($(pwd))를 컨테이너의 /app 폴더와 연결하여 백그라운드 실행
taek@OHs-MacBook-Air bind-test % docker run -d --name bind_container -v $(pwd):/app ubuntu sleep 3600
01183b22b062e666045b87c37edb926601c545650aef1fe6f1e59a6984e67b05

# 컨테이너 내부의 /app/memo.txt 파일에 문자열 이어쓰기(>>) 명령 하달
taek@OHs-MacBook-Air bind-test % docker exec bind_container sh -c "echo ' -> Added by Docker Container!' >> /app/memo.txt"

**3. 호스트 변경 후 (결과 증거)**
# 호스트(맥북)에서 다시 원본 파일을 열어 내용 확인
taek@OHs-MacBook-Air bind-test % cat memo.txt
This is Host(MacBook) Data.
 -> Added by Docker Container!
```

---

### 5. 🛠️ 트러블슈팅 (Troubleshooting)

**사례 1: 컨테이너 이름 중복 충돌 (Conflict Error)**
* **문제 발생:** `docker run -it --name my_ubuntu ubuntu bash` 실행 시 `Conflict. The container name "/my_ubuntu" is already in use...` 에러 발생.
* **원인 가설:** 이전에 생성했다가 종료(`exit`)한 컨테이너가 삭제되지 않고 `my_ubuntu`라는 이름을 계속 점유하고 있을 것이다.
* **확인:** `docker ps -a` 명령어로 확인 결과, `Exited` 상태인 기존 `my_ubuntu` 컨테이너 발견.
* **해결 및 대안:** `docker rm my_ubuntu` 명령어로 기존에 종료된 컨테이너를 완전히 삭제한 후 다시 `run` 명령어를 실행하여 정상 진입 완료.

**사례 2: zsh 환경에서 느낌표(!) 인식 오류**
* **문제 발생:** `echo "<h1>...Server!</h1>" > index.html` 실행 시 `zsh: event not found: </h1>` 에러 발생.
* **원인 가설:** Mac의 기본 쉘인 zsh에서 느낌표(`!`)를 일반 문자열이 아닌 '명령어 히스토리 호출(History Expansion)' 특수 문자로 해석하여 발생한 문법 오류일 것이다.
* **확인:** zsh 공식 문서 및 동작 원리 확인 결과, 큰따옴표 안에서도 `!`가 특수문자로 작동함을 확인.
* **해결 및 대안:** 느낌표 앞에 이스케이프 문자(역슬래시 `\`)를 붙여 `Hello Codyssey\!` 와 같이 일반 문자로 강제 인식시켜 해결 완료.