# 서론

오늘은 RN에서 IOS 네이티브 모듈과 직접 통신하는 방법에 관해서 알아보도록 하겠습니다. 

# 본론

먼저, Xcode 상에서 새로운 Object-C 파일을 생성해줍시다. XCode 상, `File -> New -> File`로 접근할 수 있습니다.


![New_File](../images/New_File.png)

### Object-C 모듈/메서드 내보내기

```c
// RTCCalendarModule.h
#import <React/RCTBridgeModule.h>
@interface RCTCalendarModule : NSObject <RCTBridgeModule>
@end
```

헤더파일에 RCTBridgeModule 모듈을 항상 불러와줍니다. 그래야, JS와 Object-C 간에 통신이 가능합니다. 

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

이제, 간단하게 RN에서 받아온 값을 출력하는 코드를 작성하였습니다. 
특이한 점은 작성한 메서드와 모듈은 RCT_EXPORT_MODULE, RCT_EXPORT_METHOD로 직접 Export 해줘야 한다는 점입니다.

### Javascript 파일에서 불러오기

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

react-native에서 제공하는 NativeModules에 Object-C 모듈 명을 적어준 뒤, Xcode 상에서 IOS 앱 빌드를 재실행해줍니다. 만약 제대로 실행되지 않으면 Xcode의 캐쉬를 제거한 후 다시 시도해보아야 합니다. 

### Object-C 파일 대신 Swift 파일 띄우기

이번엔 Swift 파일을 띄워보도록 하겠습니다. Object-C 파일을 띄울 때에 비해, 하나의 과정이 추가됩니다. Swift를 사용하여 Xcode 프로젝트를 빌드하려면, 기본 앱 프로젝트에 `Swift 코드`와 `브리징 헤더` 자체가 포함되어야 합니다. 브리징 헤더는 Object-C 파일을 Swift에 노출시키는 역할을 합니다.

```C
// CalendarManager-Bridging-Header.h
#import <React/RCTBridgeModule.h>

```

마찬가지로 위의 경우처럼 헤더 파일을 설정해줍니다. 

```C
#import <React/RCTBridgeModule.h>

@interface RCT_EXTERN_MODULE(CalendarManager, NSObject)

RCT_EXTERN_METHOD(addEvent:(NSString *)name location:(NSString *)location date:(nonnull NSNumber *)date)

@end
```

Swift 모듈과 메서드를 Bridge 파일에서 export 해줌으로써, Object-C에서 Swift 파일을 불러올 수 있게 됩니다. 

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

브릿지 헤더의 이름은 아래와 같이, X-Code에서도 수정해줘야 합니다. `Target -> Built Settings -> Objective-C Bridging Header`로 접근할 수 있습니다. 


![Swift-Header](../images/Swift-Header.png)

저는 RN-Bridge-Manager란 이름으로 설정해줬습니다.

### Javascript 파일에서 불러오기

```javascript
import React from 'react';
import { NativeModules, Button } from 'react-native';
const { CalendarManager } = NativeModules;

const NewModuleButton = () => {
  const onPress = () => {
    CalendarManager.addEvent('Swift Test', 'Test Location', 2022);
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

앞서 작성했던 Swift 파일을 Javascript 파일에서 불러오기 위해, Module 명을 수정해줍니다. 이 때, swift 파일에서 Object-C로 등록할 때 사용한 이름을 써줍니다. 

# 마무리

이상으로 간단하게 RN에서 IOS Native Module과 통신하는 방법에 알아보았습니다. 읽어주셔서 감사합니다. 

# 참고한 사이트

[ReactNative에서 IOS Native Modules 사용하기](https://runebook.dev/ko/docs/react_native/native-modules-ios)
[ReactNative에서 IOS Native Modules 사용하기 (영상)](https://www.youtube.com/watch?v=DREQwNb99l0&ab_channel=UnsureProgrammer)