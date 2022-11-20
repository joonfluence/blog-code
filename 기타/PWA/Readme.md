# 서론

오늘은 웹 코드를 바탕으로 앱을 만들 수 있는 기술인 PWA(Progressvie Web App)에 관해서 알아보도록 하겠습니다.

### 장점

- 만들기 쉽다.
  - 이미 만든 웹사이트나 웹어플리케이션이 있다면 데스크탑이나 모바일에서도 동작할 수 있도록 만들 수 있음.
- 접근성이 높다.
  - 간단하게 배포하고
- 네이티브 앱의 기능을 사용할 수 있다.
  - 제약이 있지만, 푸쉬 알림 같이 앱 상 대부분의 기능들을 제공한다.

### 단점 (제약사항)

- 네이티브 앱에 비해, 기능상 제약이 존재한다.
  - IOS Push Notification 기능이 안됨 (2022년 WWDC 발표에 따르면, 2023년 추가 예정)

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

### 추가기능

- App Shortcut
  - manifest.json 파일에 shortcuts를 추가해주면 된다.
- Contact Picker
  - 연락처 정보를 통해, 앱 초대가 가능하다. 
- GEOLOCATION 
  - 유저의 현재 위치를 알 수 있다. 
- Device Motion
  - 유저 핸드폰 각도와 같은 정보들을 알 수 있다.
- External Devices
  - Bluetooth, NFC, USB, HID 등에 연결 가능하다. 
- Idle Detection 
  - 유저가 설치한 앱을 사용하고 있는지 파악 가능하다.
- File System
  - 브라우저 제한이 있을 수 있음
- Web Share & Web Share Target
  - 


### PWA를 위한 4가지 스텝

- 이미 만들어진 웹 어플리케이션이 있어야 한다.
- 보안을 위해, HTTPS 등록이 되어 있어야 한다.
- Application Manifest 등록이 되어 있어야 한다.
  - 다양한 기기들에 애플리케이션 설치를 할 수 있도록 도와준다.
- Service Worker가 등록되어야 합니다.
  - Service Worker란 자바스크립트로 작성할 수 있는 스크립트이며, 어플리케이션에서 서버와 데이터를 주고 받을 때 중간에서 그 모든 요청들을 통제하고 관리할 수 있다.

# 참고한 사이트

[드림코딩 앨리 - PWA편](https://www.youtube.com/watch?v=FEBkne7Nyu4&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9)
[PWA - 7 Web Features You Didn’t Know Existed](https://www.youtube.com/watch?v=ppwagkhrZJs&ab_channel=Fireship)
[PWABuilder Github](https://github.com/pwa-builder/pwabuilder-web/blob/V2/src/assets/next-steps.md)
[Web.dev](https://web.dev/install-criteria/)
