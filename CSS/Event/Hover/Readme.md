# 서론

### 대상독자

HTML과 CSS 만으로 화면에서 태그를 감췄다 보였다 처리해주고 싶은 개발자

# 본론

### 구현방법

먼저, 정답은 아주 간단합니다! CSS 가상선택자 :hover 속성을 활용하면 충분히 구현할 수 있습니다. 우선 먼저, css에서 label 태그로 마우스 오버 대상 태그를 생성하고 보여줄 목록을 ul 태그로 설정합니다. 메뉴에 대한 기본 속성을 display : none으로, :hover 상태를 display: block 등으로 지정해주면 됩니다. 이게 끝입니다!!

만약 리액트와 같은 UI 라이브러리를 사용한다면, 컴포넌트 onMouseEnter와 onMouseLeave 속성을 사용하면 됩니다. 리액트에서의 방법은 추후 다뤄보도록 하겠습니다.

### 실제 코드

이상입니다! 읽어주셔서 감사합니다.