# 기본 사용법

git init : 작업 중인 폴더에서 git을 사용할 수 있도록 해줌. 

git add . : 폴더 내의 모든 파일을 **git staging area**에 추가해줌. ****

git commit -m "commit message" : 커밋과 동시에 커밋 메세지 작성함. 

git remote add origin repostory주소 : repository 주소를 origin 이름으로 등록함 

git push -u origin master : origin이란 원격 저장소에 master branch에 푸시함. 

# 버젼관리 (변경사항 등록 및 삭제)

**working directory** : 로컬 레포지토리 등록이 되면 자동으로 변경사항이 추적되며, 그 내용은 이곳에 기록된다. 

**staging area** : 버전관리를 하고 싶은 파일/폴더들을 보관하는 곳이다. 

git add [파일명] 

**repository** : 버젼관리 등록이 된 파일/폴더들을 보관하는 곳이다. 

git commit -m "commit message" 

git commit -am : 한번 트랙된 파일들을 add와 commit을 동시해 처리해줌. 

### 버젼 읽기

git status : working directory의 변경사항들을 목록화해준다.

git log : commit 히스토리 확인 

git log -p : commit 간에 파일의 바뀐 내용을 보여줌

git log —stat : 파일들의 변경 여부를 알려준다.

### 버젼 수정하기

git checkout [commit-id] : 해당 버젼으로 변경된다. 

git checkout master : 최신 버젼의 정보로 되돌아간다. 

### 버젼 삭제하기

git reset HEAD^ : 바로 이전 커밋 삭제하기

--soft : index 보존(add O, staged O), 워킹 디렉토리의 파일 보존. 

--hard : index 취소(add X, unstaged), 파일 삭제. 

--mixed : index 취소(add X, unstaged), 파일 보존. 

git reset -- <paths> : 이전 커밋 삭제하기 

git revert [commit-id] : 해당 커밋을 취소할 수 있다. 그러나이전 커밋들이 존재하는 상태에서 해당 커밋을 취소할 순 없다. 역순으로 전부 다 취소해야 가능하다. 대신 revert를 쓰면, 버젼정보도 보관하면서 이전 버젼으로 돌아갈 수 있다. 

### git add 취소하기(파일 상태를 Unstage로 변경하기)

아래와 같이 실수로 git add * 명령을 사용하여 모든 파일을 Staging Area에 넣은 경우,
Staging Area(git add 명령 수행한 후의 상태)에 넣은 파일을 빼고 싶을 때가 있다.
이때, git reset HEAD [file] 명령어를 통해 git add를 취소할 수 있다.

### gitignore 안먹힐 때

```shell
git rm -rf --cached .
git add .
```

### 프로젝트 불러오기 

```shell
# public repo
git clone [repository경로]

# private repo 
git clone https://닉네임@github.com/dalicious-manager/[repository경로]
```

# 참고사이트

[Git](http://git-scm.com/)
[[Git] git add 취소하기, git commit 취소하기, git push 취소하기 - Heee's Development Blog](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)
[[GitHub] remote에 이미 push한 파일 지우기](https://makemethink.tistory.com/163)