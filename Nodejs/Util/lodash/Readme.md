# 서론

## 객체 관련 메서드

lodash란 array, collection, date 등 데이터의 필수적인 구조를 쉽게 다룰 수 있게끔 하는데에 사용됩니다. 
JavaScript에서 배열 안의 객체들의 값을 handling(배열, 객체 및 문자열 반복 / 복합적인 함수 생성) 할 떄 유용합니다. 

### isEqual

```javascript
const _ = require("lodash");

const val1 = { a: "gfg" };
const val2 = { a: "gfg" };
const val3 = { a: "gag" };

// Checking for Equal Value
console.log("The Values are Equal : " + _.isEqual(val1, val2));
console.log("The Values are Equal : " + _.isEqual(val1, val3));
```

### reduce

객체 간 값 비교해서, 서로 다른 값만 객체에 담기.

```javascript
let a = {};
let b = {};

a.prop1 = 2;
a.prop2 = 2;

b.prop1 = 2;
b.prop2 = 3;

// Checking for different Value
const result = _.reduce(
  a,
  function (result, value, key) {
    return _.isEqual(value, b[key]) ? result : result.concat(key);
  },
  []
);
// diffrent value
console.log("result", result);
const obj = {};
result.forEach((item) => {
  console.log("b[item]", b[item]);
  obj[item] = b[item];
  console.log("obj[item]", obj[item]);
});

console.log(obj);
```

# 참고한 사이트 

[https://velog.io/@kysung95/%EC%A7%A4%EB%A7%89%EA%B8%80-lodash-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90](https://velog.io/@kysung95/%EC%A7%A4%EB%A7%89%EA%B8%80-lodash-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90)