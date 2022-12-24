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
- 미들웨어
  - rewrite나 redirect를 사용할 필요 없이 직접 미들웨어에서 response를 제공할 수도 있게 되었다.
  ```typescript
  // middleware.ts
  import { NextRequest, NextResponse } from 'next/server';
  import { isAuthenticated } from '@lib/auth';

  // Limit the middleware to paths starting with `/api/`
  export const config = {
    matcher: '/api/:function*',
  };

  export function middleware(request: NextRequest) {
    // Call our authentication function to check the request
    if (!isAuthenticated(request)) {
      // Respond with JSON indicating an error message
      return NextResponse.json(
        {
          success: false,
          message: 'Auth failed',
        },
        {
          status: 401,
        },
      );
    }
  }
  ```

# 참고한 사이트

[https://nextjs.org/blog/next-13#new-app-directory-beta](https://nextjs.org/blog/next-13#new-app-directory-beta)
