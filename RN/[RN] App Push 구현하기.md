# 본론

### push notification vs local push notification

채팅 메세지처럼 서버로부터 메시지를 받아와야 하는 상황에서는 전자, 서버로부터 메시지를 받아오지 않아도 되는 정해진 메세지를 전달할 때는 후자.

### Firebase 클라우드 메시징이란

Firebase 클라우드 메시징(FCM)은 메시지를 안정적으로 무료 전송할 수 있는 크로스 플랫폼 메시징 솔루션입니다. FCM을 사용하면 새 이메일이나 기타 데이터를 동기화할 수 있음을 클라이언트 앱에 알릴 수 있습니다. 이렇게 알림 메시지를 전송하여 사용자를 유지하고 재참여를 유도할 수 있습니다. 채팅 메시지와 같은 사용 사례에서는 메시지로 최대 4,000바이트의 페이로드를 클라이언트 앱에 전송할 수 있습니다.

### 의존성 설치

```
@react-native-community/push-notification-ios: 1.10.1
@react-native-firebase/analytics: 15.3.0
@react-native-firebase/app: 15.3.0
@react-native-firebase/messaging: 15.3.0
react-native-push-notification: 8.1.1
```

# 참고한 사이트

[https://firebase.google.com/docs/cloud-messaging?hl=ko](https://firebase.google.com/docs/cloud-messaging?hl=ko)
[https://dev-yakuza.posstree.com/ko/react-native/react-native-push-notification/](https://dev-yakuza.posstree.com/ko/react-native/react-native-push-notification/)
[https://github.com/zo0r/react-native-push-notification#local-notifications](https://github.com/zo0r/react-native-push-notification#local-notifications)
