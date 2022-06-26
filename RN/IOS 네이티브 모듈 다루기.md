# 서론

오늘은 RN에서 네이티브 모듈에 접근하는 방법에 관해서 알아보도록 하겠습니다. 

# 본론

### 네이티브 모듈 export/import

Xcode 상에서 새로운 Object-C 파일을 생성해줘야 합니다. 

- Object-C 모듈/메서드 내보내기

```c
// RTCCalendarModule.h
#import <React/RCTBridgeModule.h>
@interface RCTCalendarModule : NSObject <RCTBridgeModule>
@end
```

RCT_EXPORT_MODULE 로 직접 EXPORT 해줘야 함.

```c
// RCTCalendarModule.m
#import "RCTCalendarModule.h"
#import <React/RCTLog.h>
@implementation RCTCalendarModule

RCT_EXPORT_METHOD(createCalendarEvent:(NSString *)name location:(NSString *)location)
{
 RCTLogInfo(@"Pretending to create an event %@ at %@", name, location);
}

// RCTCalendarModule이라는 모듈을 내보내려면 
RCT_EXPORT_MODULE(RCTCalendarModule);

@end
```

RCT_EXPORT_METHOD 로 직접 EXPORT 해줘야 함.

- Javascript 파일에서 불러오기

```javascript
import React from 'react';
import { NativeModules, Button } from 'react-native';
const { CalendarModule } = NativeModules;

const NewModuleButton = () => {
  const onPress = () => {
    CalendarModule.createCalendarEvent('testName', 'testLocation');
  };

  return (
    <Button
      title="Click to invoke your native module!"
      color="#841584"
      onPress={onPress}
    />
  );
};

export default NewModuleButton;
```

2. Object-C 파일 대신 Swift 파일 띄우기

```C
// CalendarManagerBridge.m
#import <React/RCTBridgeModule.h>

@interface RCT_EXTERN_MODULE(CalendarManager, NSObject)

RCT_EXTERN_METHOD(addEvent:(NSString *)name location:(NSString *)location date:(nonnull NSNumber *)date)

@end
```

```C
// CalendarManager-Bridging-Header.h
#import <React/RCTBridgeModule.h>

```

헤더파일임. 

```C
#import <React/RCTBridgeModule.h>

@interface RCT_EXTERN_MODULE(CalendarManager, NSObject)

RCT_EXTERN_METHOD(addEvent:(NSString *)name location:(NSString *)location date:(nonnull NSNumber *)date)

@end
```

Swift 모듈과 메서드를 export 해줌. 

```swift
@objc(CalendarManager)
class CalendarManager: NSObject {

 @objc(addEvent:location:date:)
 func addEvent(_ name: String, location: String, date: NSNumber) -> Void {
   // print name, location, date 
   print(name, location, date);
 }

 @objc
 func constantsToExport() -> [String: Any]! {
   return ["someKey": "someValue"]
 }
}
```
타사 모듈을 만들 때 중요: Swift가 있는 정적 라이브러리는 Xcode 9 이상에서만 지원됩니다. 모듈에 포함시킨 iOS 정적 라이브러리에서 Swift를 사용할 때 Xcode 프로젝트를 빌드하려면 기본 앱 프로젝트에 `Swift 코드`와 `브리징 헤더` 자체가 포함되어야 합니다. 브리징 헤더는 Object-C 파일을 Swift에 노출시키는 역할을 합니다. 브릿지 헤더의 이름은 아래와 같이, X-Code에서 수정해줘야 합니다. 

[Swift-Header](Swift-Header.png)


# 참고한 사이트

[ReactNative에서 IOS Native Modules 사용하기](https://runebook.dev/ko/docs/react_native/native-modules-ios)
[ReactNative에서 IOS Native Modules 사용하기 (영상)](https://www.youtube.com/watch?v=DREQwNb99l0&ab_channel=UnsureProgrammer)