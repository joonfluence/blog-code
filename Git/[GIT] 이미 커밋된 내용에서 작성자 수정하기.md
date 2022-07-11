# 서론

오늘은 회사 계정으로 잘못 커밋했을 때, 커밋 계정을 변경해 github 컨트리뷰션을 남기는 방법에 관해 알아보도록 하겠습니다. 

# 해결책

1. rebase 시작

해당 git 저장소에서 연 터미널에서 아래와 같이 명령어를 입력합니다.

```shell
git rebase -i 커밋hash
```

2. 변경 원하는 커밋을 edit로 변경 및 나가기

rebase 하고 싶은 커밋의 경우 pick을 edit로 수정하면 해당 커밋을 rebase 대상으로 지정하게 됩니다. 즉, vi에디터에 등장한 모든 커밋을 rebase대상으로 보는 것이 아니라 등장한 커밋 중 edit로 된 커밋들만 rebase대상으로 보는것입니다. 

3. author 수정하기

```shell
git commit --amend --author="사용자명 <이메일>"
```

rebase 진행 모드로 변경된 후 바로 아래의 명령어를 입력하시면 됩니다.

4. 다음 커밋으로 계속 진행

```shell
git rebase --continue
```

5. 수정을 원하는 커밋일 경우 다시 rebase로 수정

```shell
git commit --amend --author="사용자명 <이메일>"
```

6. 3~5 과정 반복

7. 확인
