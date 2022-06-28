# 서론

오늘은 리액트에서 Google Geocoding을 사용하는 방법에 관해 알아보도록 하겠습니다. 

# 본론

### Geocode란?

Google Geocode API는 사용자가 주소를 입력하면, 위/경도 값으로 변환해줍니다. Reverse Geocode API도 있는데, 위/경도 값을 입력하면 사용자 주소 값으로 변환해줍니다. 

### 구글 플랫폼 가입하기 

API Key 발급을 위해 구글 플랫폼에 가입해줍니다. 그런 뒤, Maps Javascript API를 추가해줍니다. 
그런 뒤, 발급 받은 API 키를 코드 상에 입력해줍니다. 

### 코드 

```javascript
import Geocode from 'react-geocode'

Geocode.setApiKey(process.env.REACT_APP_GOOGLE_API_KEY)
Geocode.setLanguage('en')
Geocode.setRegion('es')
Geocode.enableDebug()

const GoogleMap = async (currentAddr) => {
  return Geocode.fromAddress(currentAddr)
    .then( response => {
      const { lat, lng } = response.results[0].geometry.location;
      return {lat, lng}
    })
    .catch(err => console.log(err))
}

export default GoogleMap 
```

코드는 위와 같습니다.

# 마무리 

오늘은 이상으로 리액트에서 Google Geocoding API 활용 방법에 알아보았습니다. 읽어주셔서 감사합니다. 

# 참고한 사이트

[[React.js] React로 Google Map Reverse Geocoding하기](https://hocheon.tistory.com/87)