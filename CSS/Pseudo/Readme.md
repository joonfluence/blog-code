### 서론

CSS를 사용하다보면, 다양한 Pseudo-Element를 사용하여 스타일링 해야 할 때가 있습니다. 오늘은 CSS Pseudo-Element(수도) 중 `::before`와 `::after` 관해 알아보는 시간을 갖도록 하겠습니다. 

### Pseudo-Element란

Pseudo-element란 HTML element의 특정 부분에 스타일을 지정할 수 있도록 selector에 추가되는 키워드입니다. selector 만으로 원하는 element를 지정할 수 없을 때 사용합니다. 잠깐, selector에 대해서 모르신다구요?
대표적인 selector가 class selector죠. 아래 예시의 `.box`와 같이요. selector에 대해서 더 알고 싶으신 분들은 [아래 링크](https://www.w3schools.com/cssref/css_selectors.php)를 참고 부탁드립니다. 

```html
<div>
  <div class="box">
    box
  </div>
</div>
```

```css
.box {
  width: 50px;
  height: 50px;
  color: red;
}
```

자, 사전 설명은 이정도로 해두고 오늘의 주제인 ::before와 ::after에 대해서 알아보죠. 바로 구체적인 사용 예시에 관해서 알아보도록 하겠습니다.

### ::before와 ::after의 대표적인 용례

::before는 selector로 지정된 요소 전에 삽입하는 요소입니다. 그럼 ::after는요? 반대겠죠. 
예를 들어서, `.figure`이란 selector로 지정한 ul 태그가 있습니다. `.figure::before`를 사용하게 되면, 아래 예시에서처럼 ul 태그의 자식으로 나열된 li 태그보다도 앞 부분의 요소를 지정할 수 있습니다. 저는 여기에 삼각형을 넣어보겠습니다. 반대로 `.comment::after`를 사용하게 되면 아래 예시에서처럼 ul 태그의 자식으로 나열된 li 태그의 뒷 부분의 요소를 지정할 수 있습니다. 여기에 주석을 의미하는 `//`를 넣어보겠습니다. 

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="ExOYVPY" data-user="joonfluence" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/joonfluence/pen/ExOYVPY">
  Pseudo Classes ::before and ::after</a> by joonfluence (<a href="https://codepen.io/joonfluence">@joonfluence</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### 그 외 예시 

그 외에도 다양한 용도로 사용할 수 있습니다. 

1. 추가적인 스타일링 

::before와 ::after의 주요 사용 사례 중 하나는 요소에 장식 내용을 추가하는 것입니다. 여기에는 요소의 시각적 모양을 개선하는 아이콘, 배경 이미지 또는 장식 테두리가 포함될 수 있습니다. 위의 예시를 조금 수정하면 멋진 아이콘을 만들 수 있겠죠!

2. float 제거하기 

float를 제거하기 위해, clearfix란 걸 해줘야 하는데, ::after 태그를 활용하여 쉽게 할 수 있습니다. 

3. Form Input의 스타일링

인풋 안에 아이콘을 넣는 등 스타일링을 추가해줄 수 있습니다. 

4. CSS 툴팁 

::before와 ::after를 사용하여 Javascript에 의존하지 않고 사용자 경험을 향상시키는 CSS 전용 툴팁을 만들 수 있습니다. 

### 결론

이상으로 ::before와 ::after에 관해 알아보았습니다. 오늘도 읽어주셔서 감사합니다. 더 좋은 글로 찾아뵙겠습니다. 