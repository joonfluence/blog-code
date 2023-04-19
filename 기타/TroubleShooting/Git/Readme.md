### 문제 상황

- git reset HEAD 명령을 쳐서 git add 기록을 unstaged 상태로 되돌리려는 상황에서 다음과 같은 에러가 발생했다.

```
fatal: ambiguous argument 'HEAD': unknown revision or path not in the working tree.
```

### 해결 방법

- git update-ref -d HEAD
- Do not use rm -rf .git or anything like this as this will completely wipe your entire repository including all other branches as well as the branch that you are trying to reset.
