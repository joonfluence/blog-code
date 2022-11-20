### 변경된 사항

- 폴더 구조의 변경
  - pages 폴더 -> app
  - page라는 이름의 파일이 꼭 들어가야 함. `ex) page.js`
- SSR 함수 문법 변경
  - fetch 문법 사용하려면 use()로 사용 가능함.
  - getServerSideProps -> fetch(URL, { cache: 'no-store' })
- 로딩
  - loading.[js|ts] 파일에 적어줌.
- 에러처리
  - error.[js|ts] 파일에 적어줌.

# 참고한 사이트

[https://nextjs.org/blog/next-13#new-app-directory-beta](https://nextjs.org/blog/next-13#new-app-directory-beta)
