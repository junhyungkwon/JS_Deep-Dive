# strict mode

# 20.1 strict mode란?

```js
function foo() {
  x = 10;
}
foo();
console.log(x); // ?
```

미선언 x에 할당, 할당을 위해 자바스크립트 엔진은 변수 선언을 스코프 체인을 통해 검색

1. foo 함수 스코프에서 변수 선언 검색 -> 없음, 실패
2. 상위 스코프 검색
3. 전역 스코프 검색
4. 선언이 존재하지 않으므로 ReferenceError 발생 -> 암묵적으로 전역 객체에 x프로퍼티 동적 생성(암묵적 전역)

암묵적 전역은 오류 발생 가능성 큼
반드시 var, let, const 키워드 사용하여 변수를 선언한 다음 사용해야 한다.

오류를 줄여 안정적인 코드를 생산하기 위해서 근본적인 접근 필요
잠재적인 오류를 발생시키기 어려운 개발 환경을 만들고 그 환경에서 개발하는 것이 좀 더 근본적인 해결책

이를 지원하기 위해 ES5부터 strict mode(엄격 모드)가 추가
strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용
오류 가능성 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러 발생
ESLint 같은 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다.

린트 도구는 정적 분석(static analysis)기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여
문법적 오류만이 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅해주는 유용한 도구

```js
function foo( ) {
    x = 10; // 'x' is not defined. eslint(no-undef)
```

1. 코딩 컨벤션을 설정 파일 형태로 정의하고 강제
2. 필자는 strict mode보다 린트 도구의 사용을 선호
3. ES6에서 도입된 클래스와 모듈은 기본적으로 strict mode가 적용

[ESLint를 사용하는 방법](https://poiemaweb.com/eslint)

# 20.2 strict mode의 적용

전역의 선두 또는 함수 몸체의 선두에 'use strict'; 추가
전역의 선두 -> 스크립트 전체 적용
함수 몸체의 선두 -> 해당 함수와 중첩 함수에 적용

```js
// 1. 전역 선두
"use strict";
function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();

// 2. 함수 몸체의 선두
function foo() {
  "use strict";
  x = 10; // ReferenceError: x is not defined
}
foo();

// 미적용
function foo() {
  x = 10; // 에러들 발생시키지 않는다.
  ("use strict");
}
foo();
```

# 20.3 전역에 strict mode를 적용하는 것은 피하자

전역에 적용한 strict mode는 스크립트 단위로 적용

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      "use strict";
    </script>
    <script>
      x = 1; // 에러가 발생하지 않는다.
      console.log(x); // 1
    </script>
    <script>
      "use strict";
      y = 1; // ReferenceError: y is not defined
      console.log(y);
    </script>
  </body>
</html>
```

스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용
하지만 strict mode 혼용은 오류를 발생시킬 수 있음

외부 서드파티 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하는 것은 바람직하지 않음

이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용한다.

```js
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  "use strict";
  // Do something...
})();
```

# 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

함수 단위로 strict mode를 적용할 수도 있다.
어떤 함수는 strict mode를 적용하지 않는 것은 바람직하지 않으며
모든 함수에 일일이 strict mode를 적용하는 것은 번거로운 일

strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제

```js
(function () {
  // non-strict mode
  var let = 10; // 에러가 발생하지 않는다
  function foo() {
    "use strict";
    let = 20; // SyntaxError: Unexpected s tric t mode reserved word
  }
  foo();
})();
```

strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직

# 20.5 strict mode가 발생시키는 에러

strict mode를 적용했을 때 에러가 발생하는 대표적인 사례

## 20.5.1 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

```js
(function () {
  "use strict";
  x = 1;
  console.log(x); // ReferenceError: x is not defined
})();
```

## 20.5.2 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.

```js
(function () {
  "use strict";
  var x = 1;
  delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.
  function foo(a) {
    delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
})();
```

## 20.5.3 매개변수 이름의 중복

중복된 매개변수 이름을사용하면 SyntaxError가 발생한다.

```js
(function () {
  "use strict";
  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
})();
```

## 20.5.4 with 문의 사용

with 문을 사용하면 SyntaxError가 발생한다.
with 문은 전달된 객체를 스코프 체인에 추가한다.
with 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해짐
-> 성능과 가독성이 나빠지는 문제
따라서 with 문은 사용하지 않는 것이 좋다.

```js
(function () {
  "use strict";
  // SyntaxError: Strict mode code may not include a with statement
  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

# 20.6 strict mode 적용에 의한 변화

## 20.6.1 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this 에 undefined가 바인딩
생성자 함수가 아닌 일반함수 내부에서는 this를 사용할 필요가 없기 때문에 에러X

```js
(function () {
  "use strict";
  function foo() {
    console.log(this); // undefined
  }
  foo();
  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
})();
```

## 20.6.2 arguments 객체

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```js
(function () {
  "use strict";
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;
  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0：1, length：1 }
})(1);
```
