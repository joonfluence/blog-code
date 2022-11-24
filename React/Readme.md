### 목차

- 리액트를 쓰는 이유와 동작원리
- Node.js와의 관계 
### 리액트를 쓰는 이유와 동작원리

 리액트는 SPA이다. SPA를 쓰는 이유는 동적웹의 UX가 좋지 않기 때문이며, SPA에서는 페이지 전환이 일어나지 않기 때문에 한 페이지에서 정보들을 탐색할 수 있어 사용성이 좋다. 단점은  SPA의 초기 구동 속도가 느리다는 점이다. 그 까닭은 그만큼 많은 용량을 클라이언트 사이드 즉 브라우저에서 로드해야 되기 때문이다. 그만큼 서버측에서 해야 할 일은 줄어들게 되고, 미리. 파일을 받아옴으로써 브라우저는 데이터를 로드하는데, 필요한 정보들만 받아와서 렌더링해주면 된다. 여기서 React를 쓰게 되면, 페이지 깜빡임 없이도 새롭게 화면을 렌더링해주므로 사용자 경험은 향상되게 된다.

 리액트는 데이터가 변할 때마다 어떤 변화를 줄 것인지 고민하는 것이 아니라 그냥 기존 뷰를 날려 버리고 처음부터 새로 렌더링하는 방식이다. 기존 방식에 비해 애플리케이션 구조가 매우 간단하고, 작성해야 할 코드양도 많이 줄어든다. 

 리액트의 주요 특징 중 하나는 Virtual DOM을 사용하는 것이다. DOM 대신 이를 쓰는 까닭은 DOM을 사용하면, 변화가 일어났을 때마다 웹 브라우저가 CSS를 다시 연산하고, 레이아웃을 구성하고, 페이지를 리페인트한다. 이 과정에서 시간이 허비된다. 그러나 이 방식에선 DOM 업데이트를 추상화함으로써 DOM 처리 횟수를 최소화하고 효율적으로 진행한다. 특히 지속적으로 데이터가 변화하는 대규모 애플리케이션을 구축할 때, 효과적이다. 

### 리액트와 node js의 관계

 React를 통한 **대규모 [SPA](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80_%ED%8E%98%EC%9D%B4%EC%A7%80_%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98) 개발**을 위해서는 몹시 많은 정적 파일이 필요하고, (.js, .css, .png 등) 이를 [번들링](https://docs.microsoft.com/en-us/aspnet/mvc/overview/performance/bundling-and-minification) 할 필요가 있습니다.→ webpack을 사용합니다. 

 ES6를 해석할 수 없는 IE11 등 [비非 모던 웹 브라우저에 대한 지원](https://www.w3schools.com/js/js_versions.asp)이 필요합니다.→ Babel을 사용합니다. 고로 React와 Node.js의 관계는.. 쫌 건너건너라고 할 수 있죠  

 또한 React의 [의존성 관리](https://devopedia.org/dependency-manager)는 [npm](https://www.npmjs.com/)을 사용하는게 가장 일반적 입니다.따라서 npm을 쓰려면 역시 Node.js가 필요하게 됩니다! 그리고 React의 부트스트랩인 [create-react-app](https://create-react-app.dev/)을 사용하기 위해서, 역시 Node.js가 필요합니다!!