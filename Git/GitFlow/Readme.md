# 본론

오늘은 Git 브랜치 관리 전략 중 하나인 GitFlow를 사용하는 방법에 관해서 알아보도록 하겠습니다.

## git flow 활용법

1. git clone [원본 레포지토리]
2. git flow init : git flow를 시작한다.
3. git pull origin develop : 먼저 sevelop branch의 내용을 최신화한다.
4. git flow feature start “카드번호” : 새로운 feature 개발을 위해 branch를 판다.
5. git add & git commit -m “카드번호 커밋내용” : 브랜치에 커밋을 한다.
6. git pull origin develop --rebase (아래와 같음)
7. git pull origin develop:develop
8. git flow feature finish 지라카드번호

- 여기서 `merge가 일어난다`. 그리고 `생성했던 branch는 삭제된다`.

9. git push origin develop : 원격저장소에 내용을 반영한다.

## 실제 예시

1. upstream/feature-user 브랜치에서 작업 브랜치(bfm-100_login_layout)를 생성합니다.

   > (feature-user)]$ git fetch upstream(feature-user)]$ git checkout -b bfm-100_login_layout --track upstream/feature-user

2. 작업 브랜치에서 소스코드를 수정합니다. (뚝딱뚝딱 :hammer:)
3. 작업 브랜치에서 변경사항을 커밋합니다. (보통은 vi editor에서 커밋 메세지를 작성 함)

   > (bfm-100_login_layout)]$ git commit -m "BFM-100 로그인 화면 레이아웃 생성"

4. 만약 커밋이 불필요하게 어려 개로 나뉘어져 있다면 squash를 합니다. (커밋 2개를 합쳐야 한다면)

   > (bfm-100_login_layout)]$ git rebase -i HEAD~2

5. 작업 브랜치를 upstream/feature-user에 rebase합니다.

   > (bfm-100_login_layout)]$ git pull --rebase upstream feature-user

6. 작업 브랜치를 origin에 push합니다.

   > (bfm-100_login_layout)]$ git push origin bfm-100_login_layout

7. Github에서 bfm-100_login_layout 브랜치를 feature-user에 merge하는 Pull Request를 생성합니다.
8. 같은 feature를 개발하는 동료에게 리뷰 승인을 받은 후 자신의 Pull Request를 merge합니다. 만약 혼자 feature를 개발한다면 1~2명의 동료에게 리뷰 승인을 받은 후 Pull Request를 merge합니다.

# 출처

[https://uroa.tistory.com/106](https://uroa.tistory.com/106)
[https://techblog.woowahan.com/2553/](https://techblog.woowahan.com/2553/)
