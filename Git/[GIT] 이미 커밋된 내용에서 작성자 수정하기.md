# 서론

오늘은 회사 계정으로 잘못 커밋했을 때, 커밋 계정을 변경해 github 컨트리뷰션을 남기는 방법에 관해 알아보도록 하겠습니다. 

# 본론

## 해결책

1. rebase 시작

잘못된 커밋 직전 커밋의 hash를 확인해줍니다.

그리고 해당 git 저장소에서 연 터미널에서 아래와 같이 명령어를 입력합니다.

```shell
git rebase -i 커밋hash
```

예를 들어, 아래와 같이 입력할 경우 48010bb 커밋 이후부터의 모든 커밋들을 rebase대상으로 지정하게 됩니다.

```shell
git rebase -i 48010bb
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

커밋을 푸쉬하여 github에 반영되었는지 확인해줍니다. 

![이미 커밋된 내용에서 작성자 수정하기](./%5BGIT%5D%20%EC%9D%B4%EB%AF%B8%20%EC%BB%A4%EB%B0%8B%EB%90%9C%20%EB%82%B4%EC%9A%A9%EC%97%90%EC%84%9C%20%EC%9E%91%EC%84%B1%EC%9E%90%20%EC%88%98%EC%A0%95%ED%95%98%EA%B8%B0.png)

# 참고한 사이트

[https://jojoldu.tistory.com/120](https://jojoldu.tistory.com/120)

