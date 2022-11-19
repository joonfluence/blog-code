### PWA

Progressvie Web App의 약자, 웹 코드를 바탕으로 앱을 만들 수 있는 방법.

### 장점

- 만들기 쉽다.
  - 이미 만든 웹사이트나 웹어플리케이션이 있다면 데스크탑이나 모바일에서도 동작할 수 있도록 만들 수 있음.
- 접근성이 높다.
  - 간단하게 배포하고
- 네이티브 앱의 기능을 사용할 수 있다.
  - 제약이 있지만, 푸쉬 알림 같이 앱 상 대부분의 기능들을 제공한다.

### 단점 (제약사항)

- 네이티브 앱에 비해 제약이 생길 수 있다.

### 기술 구성요소

- responsive web design
- app manifest
- push notification
- service workers
- navtive app-like capabilities
  - installation

### 배포

- 부분적으로 앱과 같은 방식으로 배포 가능하다.
  - 구글 PlayStore에서 배포 가능하다.
  - 앱 AppStore에선 배포 불가능하다.

### 유용한 툴들

- [PWABuilder](https://www.pwabuilder.com)
  - PWA로 배포 시, 빠진 내용이 있는지 검토해줍니다.
- [Workbox](https://developer.chrome.com/docs/workbox)
  - 다양한 PWA 기능들을 위한 서비스 워커를 자동으로 만들어주는 라이브러리.
- [Maskable.app](https://maskable.app/)
  - 더 나은 사용성을 위한 어댑티브 아이콘을 디자인할 수 있는 툴.

### PWA를 위한 4가지 스텝

- 이미 만들어진 웹 어플리케이션이 있어야 한다.
- 보안을 위해, HTTPS 등록이 되어 있어야 한다.
- Application Manifest 등록이 되어 있어야 한다.
  - 다양한 기기들에 애플리케이션 설치를 할 수 있도록 도와준다.
- Service Worker가 등록되어야 합니다.
  - Service Worker란 자바스크립트로 작성할 수 있는 스크립트이며, 어플리케이션에서 서버와 데이터를 주고 받을 때 중간에서 그 모든 요청들을 통제하고 관리할 수 있다.

# 참고한 사이트

[드림코딩 앨리 - PWA편](https://www.youtube.com/watch?v=FEBkne7Nyu4&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9)
[PWABuilder Github](https://github.com/pwa-builder/pwabuilder-web/blob/V2/src/assets/next-steps.md)
[Web.dev](https://web.dev/install-criteria/)
