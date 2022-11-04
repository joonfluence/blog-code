### 대상독자

리액트 유닛 테스트를 처음으로 진행하는 프론트엔드 개발자

### 학습목표

리액트 유닛 테스트를 위한 환경을 설정할 수 있다  
원하는 테스팅 동작을 테스트 코드로 대체할 수 있다  
유닛 테스트를 하는 이유를 이해하고 설명할 수 있다

오늘은 리액트 컴포넌트 Unit 테스트를 위한 Jest와 React Testing Library 환경설정하는 방법과 주요 함수, 테스팅에 관한 일반적인 사항에 대해서 알아보도록 하겠습니다. 참고로 환경설정은 CRA로 리액트 프로젝트를 셋팅한다면, 자동으로 처리되는 과정입니다. 그러나 오늘 시간에는 리액트 유닛 테스트를 boilerplate 없이, 처음부터 설정한다는 가정에서 설정한다는 설명해보도록 하겠습니다.

### Jest와 RTL(React Testing Library)이란?

Jest는 유닛테스트를 위해 사용되는 테스팅 프레임워크입니다. 이에 반해, RTL은 테스팅 라이브러리입니다. Jest가 좀 더 포괄적인 테스팅 방법을 제공합니다. Node.js에서도 활용할 수 있죠. 후자는 DOM Testing Library로 DOM 노드(Node)를 테스트하기 위한 매운 가벼운 솔루션입니다. 과거에는 Enzyme를 많이 사용했지만, 현재는 리액트 공식문서에서도 RTL 사용을 권장하고 있습니다. 실제로 사용해보면 RTL의 사용방법이 직관적이고, 사용자 행동 중심이므로 사용하기 편리했습니다. 그래서 이번 시간에는 Jest와 RTL을 함께 사용하여 테스트하는 방법에 대해서 알아보도록 하겠습니다.

### 유닛 테스트 vs 통합테스트 vs 인수 테스트

유닛 테스트란 응용 프로그램에서 테스트 가능한 가장 작은 소프트웨어를 실행하여 예상대로 동작하는지 확인하는 테스트를 말합니다. 테스팅 방법에는 블랙박스 테스트와 화이트박스 테스트가 있는데, 유닛 테스트는 소프트웨어 내부 코드에 관련한 지식을 반드시 알고 있어야 하는 대표적인 화이트박스 테스트입니다.

통합 테스트란 단위 테스트보다 더 큰 동작을 달성하기 위해 여러 모듈들을 모아 이들이 의도대로 협력하는지 확인하는 테스트를 말합니다. 통합 테스트는 단위 테스트와 달리 개발자가 변경할 수 없는 부분(예를 들면, 외부 라이브러리)까지 묶어 검증할 때 사용합니다. DB에 접근하거나 전체 코드와 다양한 환경에서 작동하는지 확인하는 모든 테스트 과정이 이에 해당합니다.

인수 테스트는 실제 사용자가 해당 소프트웨어를 사용하는 스토리(시나리오)에 맞춰 수행하는 테스트입니다. 주로 소프트웨어를 인수할 때, 실제 사용자의 관점에서 테스트하는 경우가 많습니다.

### 리액트 유닛 테스트란

리엑트 유닛 테스팅이란 컴포넌트를 기능별로 쪼개 아주 작은 단위로 만들어, 각 요소를 독립적으로 테스팅을 하는 것을 말합니다. 이러한 유닛 테스트는 언제 하면 좋을까요? 복잡한 웹 애플리케이션 작성할 때, 필요합니다. 상당히 많은 양의 테스트를 반복해야 하는 경우, 유닛 테스트를 위한 코드를 작성해두면 반복적인 테스팅을 대체할 수 있기 때문이죠. 그러나 단순한 퍼블리싱을 할 때에는 불필요할 수 있습니다.

### 환경설정

그러면 본격적으로 환경설정부터 시작해보겠습니다.

```
npm install jest @types/jest @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

### Jest Config 방법

jest.config.json 파일을 생성하고 아래와 같이, jest 파일에 대한 환경설정을 해줍니다.

```json
{
  "moduleFileExtensions": ["js", "json", "ts", "tsx"],
  "moduleDirectories": ["node_modules", "bower_components", "src"],
  "testEnvironment": "jsdom"
}
```

package.json에 해당 구문을 추가해줍니다. Jest가 Test 파일을 찾는 방법은 {파일이름}.spec.js 등으로 설정해주면 됩니다. 혹은 {파일이름}.test.js / 으로 써줄 수도 있습니다. 편한대로 적어주시면 됩니다.

```json
"testRegex": ".*\\.spec\\.(t|j)s$",
```

### Jest 주요 함수 설명

render : DOM에 컴포넌트를 렌더링하는 함수입니다. 인자로 렌더링할 React 컴포넌트가 돌아갑니다.  
describe : 여러 관련 테스트를 그룹화합니다.  
test(it) : 개별 테스트를 수행하는 곳 입니다. 각 테스트들을 작은 문장처럼 처리하는 설명합니다. describe 메소드의 안에 정의되는 하위 함수입니다. beforeEach : 개별 테스트가 실행되기 전에 수행되는 콜백함수입니다. 주로 render 함수와 같이 쓰입니다.  
expect : 값을 테스트할 때마다 사용됩니다.  
matcher : 각각 다른 방법으로 테스트하도록 활용되는 요소입니다. jest-dom에 가면 많은 Matcher 함수들을 확인할 수 있습니다.  
tobe : 결과 값이 서로 일치하는지 확인합니다.

### RTL 주요 함수 설명

return은 RTL에서 제공하는 퀴리 함수와 기타 유틸리티 함수를 담고 있는 객체를 리턴합니다. 쿼리 함수를 이용해서 getByText(/learn react/i) 로 찾는 방법입니다.

1. 요소 가져오기

요소를 가져오는 방법에는 여러 가지 방법이 있으므로, 상황에 맞는 함수를 사용하면 됩니다. 차이점은 해당 요소를 불러오지 못하는 경우, 반환하는 에러 형식과 비동기/동기의 차이입니다. 그 외에는 차이가 없습니다. 참고로 아래 나열한 순서(getBy, queryBy, findBy)는 우선순위 순서로, 높은 우선순위 요소들을 활용하는 것이 좋습니다.

- getBy : error를 throw 합니다. 종류에는 아래와 같이 다양한 종류가 있습니다. 요소를 가져올 때, 주로 사용하는 것은 getByRole과 getByLabelText입니다.
  - screen.getByText
  - screen.getByRole
  - screen.getByLabelText
  - screen.getByPlaceholderText
  - screen.getByAltText
  - screen.getByDisplayValue
  - screen.getByTitle
- queryBy : null을 반환합니다.
- findBy : 비동기 코드를 다룰 수 있으며, error를 throw 합니다.
- fireEvent : 가상으로 유저 동작을 흉내내는 함수입니다.
  - fireEvent.input : input에 값을 입력하는 동작을 수행합니다.
  - fireEvent.click : 인자 값으로 오는 요소에 대한 클릭 동작을 수행합니다. 주로 버튼 컴포넌트와 함께 사용됩니다.
  - fireEvent.submit : 전송 동작을 수행합니다.

### 테스트 실행방법

먼저 테스트를 하기 전에, 테스트 파일 형태에 맞게 파일을 만들어줘야 합니다. /spec/, /test/ 등으로 말이죠. 그런 후, 테스트코드를 작성합니다. 마지막으로 테스트 코드를 실행하려면 아래의 커맨드를 입력해줍니다.

```
npm run jest
```

이게 끝입니다. 정말 간단하죠? Jest가 Test 파일을 찾는 방법은 {파일이름}.test.js / {파일이름}.spec.js 등으로 설정해주면 된다.

### React Native에서 설정하기

```json
{
  "preset": "react-native",
  "setupFilesAfterEnv": ["@testing-library/jest-native/extend-expect"],
  "moduleFileExtensions": ["ts", "tsx", "js", "jsx", "json", "node"],
  "testRegex": ".*\\.spec\\.(t|j)s$"
}
```

### 마무리

다음 시간에는 실제로 Test 코드를 짜보고 리액트 프로젝트에서 확인해보는 시간을 갖도록 하겠습니다. 특히, 가장 활용도가 높다고 생각되는 폼 테스팅 방법에 대해서 알아보도록 하겠습니다.

### 참고링크

[단위 테스트 vs 통합 테스트 vs 인수 테스트](https://tecoble.techcourse.co.kr/post/2021-05-25-unit-test-vs-integration-test-vs-acceptance-test/)  
[리액트 유닛 테스트](https://velog.io/@goodenough/React-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%8B%A8%EC%9C%84unit%ED%85%8C%EC%8A%A4%ED%8A%B8-with-React.js-Unit-Testing-Enzyme-and-Jest-92wgndmz)
