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

```javascript
import PushNotification from 'react-native-push-notification';
...
export default {
  register: () => {
    PushNotification.configure({
      onNotification: function(notification) {
        notification.finish(PushNotificationIOS.FetchResult.NoData);
      },
      popInitialNotification: true,
    });
    _registerLocalNotification();
    ...
  },
  ...
};
```

register()라는 함수를 실행 시키면 PushNotification을 초기화한 후, 앱(App) 자체 알림(Local Push Notification)을 등록하는 함수를 호출합니다.

```javascript
const _registerLocalNotification = () => {
  PushNotification.setApplicationIconBadgeNumber(0);
  PushNotification.cancelAllLocalNotifications();

  const messages = [
    ...
  ];
  const message = messages[Math.floor(Math.random() * messages.length)];

  let nextHour = new Date();
  nextHour.setDate(nextHour.getDate() + 1);
  nextHour.setHours(nextHour.getHours() - 1);


  PushNotification.localNotificationSchedule({
    /* Android Only Properties */
    vibrate: true,
    vibration: 300,
    priority: 'hight',
    visibility: 'public',
    importance: 'hight',

    /* iOS and Android properties */
    message, // (required)
    playSound: false,
    number: 1,
    actions: '["OK"]',

    // for production
    repeatType: 'day', // (optional) Repeating interval. Check 'Repeating Notifications' section for more info.
    date: nextHour,

    // test to trigger each miniute
    // repeatType: 'minute',
    // date: new Date(Date.now()),

    // test to trigger one time
    // date: new Date(Date.now() + 20 * 1000),
  });
};
```

앱(App) 자체 알림(Local Push Notification)을 등록하기 위해 \_registerLocalNotification 함수를 실행 시키면,

```javascript
PushNotification.setApplicationIconBadgeNumber(0);
PushNotification.cancelAllLocalNotifications();
```

뱃지(Badge)를 초기화 시키고, 기존에 등록되어 있는 알림(Notification)을 전부 제거합니다. 기존에 등록된 알림(Notification)을 전부 제거한 이유는 앱이 실행될 때마다 등록을 시키기 때문에 중복되서 같은 메세지가 발송되는 문제가 있었습니다. AsyncStorage를 통해 제어하려고 하였지만, 그냥 전부 지우고 새로 등록하는 방식을 선택했습니다.

```javascript
PushNotification.localNotificationSchedule({
  /* Android Only Properties */
  vibrate: true,
  vibration: 300,
  priority: "hight",
  visibility: "public",
  importance: "hight",

  /* iOS and Android properties */
  message, // (required)
  playSound: false,
  number: 1,
  actions: '["OK"]',

  // for production
  repeatType: "day", // (optional) Repeating interval. Check 'Repeating Notifications' section for more info.
  date: nextHour,

  // test to trigger each miniute
  // repeatType: 'minute',
  // date: new Date(Date.now()),

  // test to trigger one time
  // date: new Date(Date.now() + 20 * 1000),
});
```

앱 자체 알림(Local Push Notification)을 등록합니다. 다양한 옵션이 있습니다. 아래에 링크를 통해 확인해 주세요.

```javascript
import LocalNotification from '~/Util/LocalNotification';

export default class LevelScreen extends React.Component<Props, State> {
    ...
    componentWillUnmount() {
        LocalNotification.unregister();
        ...
    }
    componentDidMount() {
        LocalNotification.register();
        ...
    }
}
```

# 참고한 사이트

[https://firebase.google.com/docs/cloud-messaging?hl=ko](https://firebase.google.com/docs/cloud-messaging?hl=ko)
[https://dev-yakuza.posstree.com/ko/react-native/react-native-push-notification/](https://dev-yakuza.posstree.com/ko/react-native/react-native-push-notification/)
[https://github.com/zo0r/react-native-push-notification#local-notifications](https://github.com/zo0r/react-native-push-notification#local-notifications)
