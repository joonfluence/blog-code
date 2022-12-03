# 서론

오늘은 GIT을 통해 버젼관리를 하는 방법에 관해, 혼자 작업하는 상황을 가정하고 설명하겠습니다.

### 버젼관리 (변경사항 등록 및 삭제)

![git_three_tree](git_three_tree.png)

Local Repository는 working directory, staging area, repository 이렇게 3개의 트리로 구성됩니다.

- **working directory** : 로컬 레포지토리 등록이 되면 자동으로 변경사항이 추적되며, 그 내용은 이곳에 기록되는 곳을 말합니다.
- **staging area** : 버전관리를 하고 싶은 파일/폴더들을 보관하는 곳입니다.
- **repository** : 버젼관리 등록이 된 파일/폴더들을 보관하는 곳입니다.

### 기본 사용 방법

- git init
  - 작업 중인 폴더에서 git을 사용할 수 있도록 해줍니다.
- git add [파일명]
  - 폴더 내의 모든 파일을 **git staging area**에 추가해줍니다.
  - git add . 입력하면, 디렉토리 내 모든 파일을 staging area로 이동시킵니다.
- git commit
  - -m "commit message"
    - 커밋과 동시에 커밋 메세지 작성합니다.
  - -am
    - 한번 트랙된 파일들을 add와 commit을 동시해 처리합니다.
- git remote add origin [repostory주소]
  - repository 주소를 origin 이름으로 등록합니다
- git push -u origin master
  - origin이란 원격 저장소에 master branch에 푸시합니다.

### 버젼 읽기

- git status
  - working directory의 변경사항들을 목록화해줍니다.
- git log
  - commit 히스토리 확인합니다.
  - -p
    - commit 간에 파일의 바뀐 내용을 보여줍니다.
  - -stat
    - 파일들의 변경 여부를 알려준다.

### 버젼 수정하기

- git checkout [commit-id]
  - 해당 버젼으로 변경된다.
- git checkout master
  - 최신 버젼의 정보로 되돌아간다.

### 버젼 삭제하기

- git reset
  - HEAD^
    - 바로 이전 커밋 삭제하기
  - --soft
    - index 보존(add O, staged O), 워킹 디렉토리의 파일 보존.
  - --hard
    - index 취소(add X, unstaged), 파일 삭제.
  - --mixed
    - index 취소(add X, unstaged), 파일 보존.
  - --[paths]
    - 현 디렉토리 영역의 커밋 삭제하기

### 특정 버젼으로 돌아가기

- git revert [commit-id]
  - 기존 커밋 내역에 영향을 주지 않으면서, 이전 커밋을 취소하기 위한 커밋을 생성한다.
  - 그러나 이전 커밋들이 존재하는 상태에서의 커밋을 취소할 순 없다. 역순으로 전부 다 취소해야 가능하다. 대신 revert를 쓰면, 버젼 정보도 보관하면서 이전 버젼으로 돌아갈 수 있다.
- git checkout [commit-id]
  - checkout을 통해서 역시, 이전 버젼으로 돌아갈 수 있다.
- git reset
  - HEAD가 가리키는 커밋을 삭제함으로써, 기존에 Repository의 히스토리를 변경합니다.

### 파일 상태를 Unstage로 변경하는 방법

실수로 git add . 명령을 사용하여 모든 파일을 Staging Area에 넣은 경우,
Staging Area(git add 명령 수행한 후의 상태)에 넣은 파일을 빼고 싶을 때가 있다.
이때, git reset HEAD [file] 명령어를 통해 git add를 취소할 수 있다.

### 작업 내용을 임시 저장하는 방법

- git stash : 모든 파일 임시저장
  - pop
    - 임시 저장 내용 복원
  - list
    - 모든 변겅점 목록을 보여줌
  - drop
    - 가장 최근에 저장한 임시 저장 내용 제거

### 프로젝트 불러오기

```shell
# public repo
git clone [repository경로]

# private repo
git clone https://닉네임@github.com/dalicious-manager/[repository경로]
```

### gitignore 안먹힐 때

```shell
git rm -rf --cached .
git add .
```

# 참고사이트

[Git](http://git-scm.com/)
[[Git] git add 취소하기, git commit 취소하기, git push 취소하기 - Heee's Development Blog](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)
[[GitHub] remote에 이미 push한 파일 지우기](https://makemethink.tistory.com/163)
