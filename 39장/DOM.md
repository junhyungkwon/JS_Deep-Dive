## 39장 Dom

- HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조이다.

## 39.1장 노드

## 39.1.1장 HTML 요소와 노드 객체

- HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.
  <img width="722" alt="스크린샷 2022-12-08 오후 11 52 55" src="https://user-images.githubusercontent.com/78072931/206478151-b523f80b-4fed-48d8-8508-80c61cc57ece.png">

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.
  <img width="606" alt="스크린샷 2022-12-08 오후 11 54 01" src="https://user-images.githubusercontent.com/78072931/206478401-73e10ff3-cf54-4c60-89ca-1001d1f0e306.png">

- 노드 객체들로 구성된 트리 자료구조를 DOM이라 한다. DOM 트리라고 부르기도 한다.

## 39.1.2 노드 객체의 타입

```
<!DOCTYPE html>
<html>
  <head>
    <mata charset="UTF-8">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

- 렌더링 엔진은 위 HTML 문서를 파싱하여 다음과 같이 DOM을 생성한다.
  <img width="649" alt="스크린샷 2022-12-09 오전 12 20 24" src="https://user-images.githubusercontent.com/78072931/206484368-b9cd62fb-1c7a-4de9-9d1a-0f12849ae78d.png">

- 중요한 노드 타입은 4가지이다.

  - 문서 노드
    DOM 트리의 최상단에 존재하는 루트노드, document 객체를 가리킨다.
    document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가르킨다
    전역 객체의 document 프로퍼티에 바인딩 되어있다. --> documnet로 참조 가능하다.
    최상단에 있기 때문에 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 한다.

  - 요소 노드
    HTML 요소를 가르키는 객체
    HTML 요소들의 중첩관계를 통해 문서의 구조를 표현한다.

  - 어트리뷰트 노드
    HTML 요소의 어트리뷰트를 가리키는 객체
    어트리뷰트 노드는 해당 요소 노드에만 연결되어 있다.

  - 텍스트 노드
    HTML 요소의 텍스트를 가리키는 객체
    해당 요소 노드의 자식 노드이며 말단 노드이다.

## 39.1.3 노드 객체의 상속 구조

- DOM을 구성하는 노드 객체는 프로토타입에 의한 상속 구조를 갖는다.
  <img width="766" alt="스크린샷 2022-12-09 오전 12 32 06" src="https://user-images.githubusercontent.com/78072931/206487392-2367d69f-6a1f-4887-b0d8-b78e2f924f5f.png">

- DOM 을 구성하는 노드 객체는 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체
- 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 가진다.
- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속
- HTML 요소의 종류에 따라 고유의 기능을 가지기도 한다.
- DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류. 즉, 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공.
- 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작 가능.

# 39.2 요소 노드 취득

- 요소 노드 취득 -> HTML 구조, 내용, 스타일 동적조작
- 텍스트 노드 : 요소 노드의 자식
- 어트리뷰트 노드 : 요소 노드와 연결

ex> HTML 문서 내의 h1 요소 텍스트 변경

1. DOM 트리 내 h1 요소 노드 취득
2. h1 요소 노드의 자식 노드 텍스트 노드 변경

## 39.2.1 id를 이용한 요소 노드 취득

Document.prototype.getElementById 메서드

- 인수 : id 어트리뷰트 값
- 하나의 요소 노드 탐색하여 반환
- Document.prototype의 프로퍼티
- 반드시 문서 노드인 document 통해 호출

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // id 값 'banana' 요소 노드 반환
      // 두 번째 li 요소가 파싱되어 생성된요소 노드 반환
      const$elem = document.getElementById("banana");

      // 취득 요소 노드 style, color 프로퍼티 값 변경
      $elem.style.color = "red";
    </script>
  </body>
</html>
```

id 값 :

- HTML 문서 내 유일값
- 공백 문자로 구분, 여러 개의 값 가질 수 X (class 어트리뷰트와 다름)
- 문서 내 중복 id 요소 -> 에러X
- 문서 내 중복된 id 요소 여러 개 존재 가능성O
- getElementById 중복 요소중 첫 번째 요소 노드만 반환

id 값을 갖는 요소 없으면, null을 반환

```html
<script>
  // ... 위 예제와 동일
  // 'grape'요소 없음, null 반환
  const $elem = document.getElementById("grape");

  // 프로퍼티 변경
  $elem.style.color = "red";
  // -> TypeError: Cannot read property 'style' of null
</script>
```

HTML 요소에 id 어트리뷰트 부여시(부수효과)

- id 값과 동일한 이름의 전역 변수 암묵적 선언
- 노드 객체가 할당

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo"></div>
    <script>
      // id값과 동일 이름 전역 변수 암묵적 선언, 해당 노드 객체 할당
      console.log(foo === document.getElementById("foo")); // true
      // 암묵적 전역으로 생성된 전역 프로퍼티는 삭제, 전역 변수는 삭제되지 않는다.
      delete foo;
      console.log(foo); // <div id="foo"></div>
    </script>
  </body>
</html>
```

> 단, id 값과 동일한 이름의 전역 변수 이미 선언시, 전역 변수에 노드 객체가 재할당X

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo"></div>
    <script>
      let foo = 1;
      // id 값과 동일한 이름의 전역 변수 이미 선언, 노드 객체 재할당X
      console.log(foo); // 1
    </script>
  </body>
</html>
```

## 39.2.2 태그 이름을 이용한 요소 노드 취득

Document.prototype/Element.prototype.getElementsByTagName 메서드 :

- 인수로 전달한 태그 이름을 갖는 모든 요소 노드 탐색, 반환
- 여러 개의 요소 노드 객체를 갖는 DOM컬렉션 객체인 HTMLCollection 객체 반환

```html
<script>
  // ... 위와 동일
  // 태그 이름이 li인 요소 노드들 HTMLCollection 객체에 담겨 반환
  // HTMLCollection 객체 : 유사 배열 객체 && 이터러블
  const $elems = document.getElementsByTagName("li");
  // 취득한 모든 요소 노드의 style , color 프로퍼티 값을 변경한다.
  // HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
  [...$elems].forEach((elem) => {
    elem.style.color = "red";
  });
</script>
```

함수 :

- 하나의 값만 반환
- 여러 개의 값을 반환 -> 배열 or 객체에 담아 반환
- HTMLCollection 객체(DOM컬렉션 객체) 유사 배열 객체 && 이터러블

HTML 문서의 모든 요소 노드 취득 -> getElementsByTagName 메서드의 인수로 \* 전달

```js
// 모든 요소 노드클 탐색하여 반환한다.
const $all = document.getElementsByTagName("*");
// -> HTMLCollection(8) [html, head, body, ul, li#apple, li#banana, li#orange, script, apple:
// li#apple, banana: li#banana, orange: li#orange]
```

getElementsByTagName :

- Document.prototype, Element.prototype 메서드 정의 2곳
- Document 내 메서드 : 문서 노드(DOM의 루트 노드), document 통해 호출
  DOM 전체에서 요소 노드 탐색 반환
- Element 내 메서드 : 특정 요소 노드를 통해 호출,
  특정 요소 노드의 자손 노드 중 요소 노드 탐색 반환

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
    <ul>
      <li>HTML</li>
    </ul>
    <script>
      // DOM 전체에서 태그 li 인 요소 노드 모두 반환
      const $lisFromDocument = document.getElementsByTagName(" l i ");
      console.log($lisFromDocument); // HTMLCollection(4) [li,li,li,li]
      // ul#fruits 요소의 자손 노드 중 태그 li 인 요소 노드 모두 반환
      const $fruits = document.getElementById("fruits");
      const $lisFromFruits = $fruits.getElementsByTagName("li");
      console.log($lisFromFruits); // HTMLCollection(3) [li,li,li]
    </script>
  </body>
</html>
```

요소X -> getElementsByTagName 빈 HTMLCollection 객체 반환

## 39.2.3 class를 이용한 요소 노드 취득

- Document.prototype/Element.prototype 메서드 정의 2곳
- 인수로 전달한 class 어트리뷰트 값 갖는 모든 요소 노드들 탐색 반환
- 인수로 전달할 class 값, 공백으로 구분, 여러 개의 class 지정가능
- getElementsByTagName 메서드와 마찬가지로 HTMLCollection 객체 반환(여러개 요소 노드)

```html
<html>
  <body>
    <ul>
      <li class="fruit apple">Apple</li>
      <li class="fruit banana">Banana</li>
      <li class="fruit orange">Orange</li>
    </ul>
    <script>
      // class 값 'fruit' 요소 노드 모두 탐색 -> HTMLCollection 객체 담아 반한
      const $elems = document.getElementsByClassName("fruit");
      // 취득한 모든 요소의 CSS color 프로퍼티 값 변경
      [...$elems].forEach((elem) => {
        elem.style.color = "red";
      });
      // class 값 'fruit apple' 요소 노드 모두 탐색 -> HTMLCollection 객체 담아 반한
      const $apples = document.getElementsByClassName("fruit apple");
      // 취득한 모든 요소의 CSS color 프로퍼티 값 변경
      [...$apples].forEach((elem) => {
        elem.style.color = "blue";
      });
    </script>
  </body>
</html>
```

getElementsByClassName 메서드

- Document.prototype 정의
  : DOM의 루트 노드인 문서 노드 전체에서 요소 노드 탐색, 반환
- Element.prototype 정의
  : 특정 요소 노드를 통해 호출, 특정 요소 노드의 자손 노드 중에서 탐색, 반환

```html
<!-- 위 동일.. -->
<div class="banana">Banana</div>
<script>
  // DOM 전체에서 class 'banana' 요소 노드들 탐색, 반환
  const $bananasFromDocument = document.getElementsByClassName("banana");
  console.log($bananasFromDocument);
  // HTMLCollection(2) [li.banana, div.banana]
  // #fruits 요소의 자손 노드 중 class 'banana' 요소 노드들 탐색, 반환
  const $fruits = document.getElementById("fruits");
  const $bananasFromFruits = $fruits.getElementsByClassName("banana");
  console.log($bananasFromFruits); // HTMLCollection [li.banana]
</script>
```

요소가 존재하지 않는 경우 빈 HTMLCollection 객체 반환

## 39.2.4 CSS 선택자를 이용한 요소 노드 취득

CSS 선택자(selector) : 스타일 적용 HTML 요소 특정

```css
/* 전체 선택자: 요소 모두 선택 */
* {
  ...;
}
/* 태그 선택자: p 태그 요소 모두 선택 */
p {
  ...;
}
/* id 선택자: id 'foo' 요소 모두 선택 */
#foo {
  ...;
}
/* class 선택자: class 'foo' 요소 모두 선택 */
.foo {
  ...;
}
/* 어트리뷰트 선택자: input 요소 중 type 어트리뷰트 'text' 요소 모두 선택 */
input[type="text"] {
  ...;
}
/* 후손 선택자: div 요소 후손 요소 p 요소 모두 선택 */
div p {
  ...;
}
/* 자식 선택자: div 요소 자식 요소 p 요소 모두 선택 */
div > p {
  ...;
}
/* 인접 형제 선택자: p 요소 형제 요소 중 p 요소 바로 뒤에 위치하는 ul 요소 선택 */
p + ul {
  ...;
}
/* 일반 형제 선택자: p 요소 형제 요소 중 p 요소 뒤에 위치하는 ul 요소 모두 선택 */
p ~ ul {
  ...;
}
/* 가상 클래스 선택자: hover 상태인 a 요소 모두 선택 */
a:hover {
  ...;
}
/* 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간 선택
일반적으로 content 프로퍼티와 함께 사용*/
p::before {
  ...;
}
```

querySelector 메서드(Document.prototype/Element.prototype)
: 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드 탐색, 반환

- 만족 요소 여러 개 : 첫 번째 요소만 반환
- 만족 요소 0 개 : null 반환
- 인수로 전달한 CSS선택자가 문법 X : DOMException 에러 발생

```html
<script>
  // html( ul li ..) 위동
  // class 'banana'인 첫 요소 탐색 반환
  const $elem = document.querySelector(".banana");
  // 취득한 요소 노드의 style.color프로퍼티 변경
  $elem.style.color = "red";
</script>
```

querySelectorAll 메서드(Document.prototype/Element.prototype)
: 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드 탐색, 반환

- 여러 개의 요소 노드 객체를 갖는 DOM컬렉션 객체인 NodeList 객체를 반환
  (NodeList 객체 : 유사 배열 객체, 이터러블)
- 만족 요소 0 개 : 빈 NodeList 객체 반환
- 인수로 전달한 CSS선택자가 문법 X : DOMException 에러 발생

```html
<script>
  // html( ul li ..) 위동
  // ul 자식 li 요소 모두 탐색, 반환
  const $elems = document.querySelectorAll("ul > li");
  // NodeList 객체에 담겨 반환
  console.log($elems);
  // NodeList(3) [li.apple,li.banana,li.orange]
  // style.color 변경, NodeList는 forEach 메서드 제공
  $elems.forEach((elem) => {
    elem.style.color = "red";
  });
</script>
```

querySelectorAll('\*') : HTML 문서의 모든 요소 노드 취득

```js
// 모든 요소 노드를 탐색하여 반환한다.
const $all = document.querySelectorAll("*");
// -> NodeList(8) [html,head,body,ul,li#apple,li#banana,li#orange,script]
```

queryselector, querySelectorAll 메서드

- Document.prototype 정의 : 루트 노드(document) 전체 노드 탐색, 반환
- Element.prototype 정의 : 특정 요소 자손 노드 탐색, 반환
- getElementsBy~~~ 보다 느리지만, 구체적인 조건으로 노드 취득 가능
- id 있을땐 getElementById, 그외에 querySelector, querySelectorAll 권장

## 39.2.5 특정 요소 노드를 취득할수 있는지 확인

Element.prototype.matches 메서드 :
인수 CSS 선택자로 특정 요소 노드 취득 확인

```html
<script>
  // 위동
  const $apple = document.querySelector(".apple");
  // $apple 노드는 '#fruits > li.apple'로 취득O
  console.log($apple.matches("#fruits>li.apple")); // true
  // $apple 노드는 '#fruits > li.banana'로 취득X
  console.log($apple.matches("#fruits>li.banana")); // false
</script>
```

Element.prototype.matches 메서드 :
이벤트 위임 사용때 유용 (40.7절 "이벤트 위임" 자세히)

## 39.2.6 HTMLCollection과 NodeList

HTMLCollection 과 NodeList(DOM 컬렉션 객체) :

- DOM API 여러개 반환
- 유사 배열 객체 && 이터러블
- for...of 순회 가능
- 배열로 변환(스프레드 문법)
- HTMLCollection : 노드 객체의 상태 변화 실시간 반영 (살아 있는 객체, live 객체)
- NodeList : 정적 상태 유지 non-live 객체(live 객체로 동작할 때 있음)

### HTMLCollection

- getElementsByTagName, getElementsByClassName 메서드
- 노드 객체의 상태 변화 실시간 반영(live 객체)

```html
<!DOCTYPE html>
<head>
  <style>
    .red {
      color: red;
    }
    .blue {
      color: blue;
    }
  </style>
</head>
<html>
  <body>
    <ul id="fruits">
      <li class="red">Apple</li>
      <li class="red">Banana</li>
      <li class="red">Orange</li>
    </ul>
    <script>
      // class 'red' 노드 보두 탐색, 반환
      const $elems = document.getElementsByClassName("red");
      // 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있다.
      console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]
      // HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경
      for (let i = 0; i < $elems.length; i++) {
        $elems[i].className = "blue";
      }
      // HTMLCollection 객체의 요소가 3개에서 1개로 변경되었다.
      console.log($elems); // HTMLCollection(1) [li.red]
    </script>
  </body>
</html>
```

getElementsEyClassName 메서드 :

- class 'red' 모두 취득
- 모든 노드 담는 HTMLCollection 객체 for 문 순회 red->blue
- 모두 blue로 변경 -> 파란색 렌더링
- 예상대로 동작 X (두번째 banana 빨간색)

동작하지 않은 이유 ($elems.length == 3, 반복 3):

1. 첫번째반복(i === 0) $elems[0] 'red' -> 'blue' (Apple -> 실시간 제거)
2. 두번패반복(i === 1) $elems[1] 'red' -> 'blue' (Orange -> 실시간 제거)
3. 세번째반복(i === 2) \$elems에 두 번째 li 요소만 남아,
   i < $elems.length==false -> 반복 종료

> 회피 방법 : for 문 역방향 순회, while

```js
// for 역방향 순회
for (let i = $elems.length - 1; i >= 0; i--) {
  $elems[i].className = "blue";
}

// while, HTMLCollection 요소 없을때까지 무한 반복
let i = 0;
while ($elems.length > i) {
  $elems[i].className = "blue";
}
```

> 간단 방법 : HTMLCollection 객체 사용X -> 배열로 변환해 사용
>
> > 유용한 배열의 고차 함수(forEach, map, filter, reduce 등) 사용가능

```js
// HTMLCollection -> 배열 순회
[...$elems].forEach((elem) => (elem.className = "blue"));
```

### NodeList

- HTMLCollection 부작용 해결 위해
- getElementsByTagName, getElementsByClassName 메서드 대신
  querySelectorAll 메서드 사용
- querySelectorAll 메서드 NodeList 객체 반환(DOM 컬렉션 객체)
- NodeList는 non-live, 정적 객체

```js
// querySelectorAll NodeList(DOM컬렉션 객체) 반환
const $elems = document.querySelectorAll(".red");
// NodeList 객체는 NodeList.prototype.forEach 메서드 상속받아 사용가능
$elems.forEach((elem) => (elem.className = "blue"));
```

NodeList 객체 :

- NodeList.prototype.forEach 메서드를 상속
- NodeList.protDtype.forEach 메서드 Array.prototype.forEach 메서드와 시용법 동일
- NodeList.prototype는 forEach, item, entries, keys, values 메서드도 제공
- 대부분의 실시간 반영 않고 정적 상태 유지 non-live 객체
- _주의_ childNodes 프로퍼티가 반환하는 NodeList 객체는 실시간 반영

```html
<script>
  // 위 동..
  const $fruits = document.getElementById("fruits");
  // childNodes 프로퍼티는 NodeList 객체(live) 반환
  const { childNodes } = $fruits;
  console.log(childNodes instanceof NodeList); // true
  // $fruits 요소의 자식 노드는 공백 텍스트 노드 포함 모두 5개
  // (39.3.1절 "공백 텍스트 노드" 자세히)
  console.log(childNodes); // NodeList(5) [text, li, text, li, text]
  for (let i = 0; i < childNodes.length; i++) {
    // removechild 메서드 : 자식 노드 DOM에서 삭제
    // (39.6.9절 "노드 삭제" 자세히)
    // removechild 메서드 호출시 childNodes 실시간 반영
    // -> 첫 번째, 세 번째, 다섯 번째요소만 삭제
    $fruits.removeChild(childNodes[i]);
  }
  // 예상과 다르게 동작
  console.log(childNodes); // NodeList(2) [li, li]
</script>
```

HTMLCollection, NodeList 객체 :

- 예상과 다르게 동작할 때 있어 다루기 까다롭고 실수하기 쉽다
- 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션 사용
- -> HTMLCollection, NodeList를 배열로 변환하여 사용 권장
- 배열은 더 다양한 기능 제공 유용한 고차 함수(forEach. map. filter, reduce 등)
- 유사 배열 객체 && 이터러블
- 스프레드 문법 || Array.from 메서드로 쉽게 변환가능

```js
const $fruits = document.getElementById("fruits");
// childNodes 프로퍼티는 NodeList 객체(live) 반환
const { childNodes } = $fruits;
// 스프레드 문법을 사용 배열로 변환
[...childNodes].forEach((childNode) => {
  $fruits.removeChild(childNode);
});
// $fruits 모든 자식 삭제
console.log(childNodes); // NodeList[]
```

## 39.3 노드 탐색

- 요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색해야 할 때가 있다.

```
<ul id ="fruits">
    <li class="aplle">Apple </li>
    <li class="banana">Banana </li>
    <li class="orange">Orange </li>
</ul>
```

- ul#fruits 요소는 3개의 자식 요소를 갖는다. 이때 먼저 ul#fruits 요소 노드를 취득한 다음, 자식 노드를 모두 탐색하거나 자식 노드 중 하나만 탐색할 수 있다. li.banana요소는 2개의 형제 요소와 부모 요소를 갖는다. 이때 먼저 li.banana 요소 노드를 취득한 후, 형제 노드를 탐색하거나 부모 노드를 탐색할 수 있다.

- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티이다. 단. 노드 탐색 프로퍼티는 setter 없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티다. 읽기 전용 접근자 프로피티에 값을 할당하면 아무런 에러 없이 무시된다.

## 39.3.1 공백 텍스트 노드

- HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성한다. 이를 공백 텍스트 노드라고 한다.

```
<DOCTYPE html>
<html>
    <body>
<ul id ="fruits">
    <li class="aplle">Apple </li>
    <li class="banana">Banana </li>
    <li class="orange">Orange </li>
    </ul>
    </body>
</html>
```

- 텍스트 에디터에서 HTML 문서에 스페이스 키, 탭 키, 엔터 키 등을 입력하면 공백 문자가 추가된다.
- HTML 문서의 공백 문자는 공백 텍스트 노드를 생성한다.
- 인위적으로 HTML 문서의 공백 문자를 제거하면 공백 텍스트 노드를 생성하지 않는다. 가독성이 좋지 않으므로 권장하지 않는다.

## 39.3.2 자식 노드 탐색

- Node.prototype.childNodes : 자식 노드 모두 탐색하여 NodeList에 담아 반환(텍스트 노드 포함)
- Node.prototype.children : 자식 노드중에서 요소 노드만 모두 탐색하여 NodeList에 담아 반환(텍스트 노드 미포함)
- Node.prototype.firstChild : 첫번째 자식 노드(텍스트 노드이거나 요소 노드)
- Node.prototype.lastChild : 마지막 자식 노드
- Element.prototype.firstElementChild : 첫번째 자식 요소 노드를 반환
- Element.prototype.lastElementChild : 마지막 자식 요소 노드를 반환

## 39.3.3 자식 노드 존재 확인

- 자식 녿가 존재하는지 확인하려면 Node.prototype.haschildNodes 메서드를 사용한다. haschildNodes 메서드는 자식 노드가 존재하면 true, 자식 노드가 존재하지 않으면 false를 반환한다. 단, haschildNodes 메서드는 childNodes 프로퍼티와 마찬가지로 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.

```
<!DOCTYPE html>
<html>
    <body>
        <ul id = 'fruits'>
        </ul>
        </body>
        <script>
        // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
        const $fruits = document.getElementByI('fruits');

        // #fruits 요소에 자식 노드가 존재하는지 확인한다.
        // haschildNodes 메서드는 텍스트 노드를 포함하여 노드의 존재를 확인한다.
        console.log($fruits.haschildNodes()); // true
        </script>
        </html>
```

- 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 hasChildNodes 메서드 대신 children.length 또는 Element 인터페이스 childElementCount 프로퍼티를 사용한다.

```
<!DOCTYPE html>
<html>
    <body>
        <ul id = 'fruits'>
        </ul>
        </body>
        <script>
        // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
        const $fruits = document.getElementByI('fruits');

        // haschildNodes 메서드는 텍스트 노드를 포함하여 노드의 존재를 확인한다.
        console.log($fruits.haschildNodes()); // true
        // 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인한다.
        console.log(!!$fruits.children.length); // 0 ->false
        // 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인한다.
        console.log(!!$fruits.childElementCount); // 0 -> false
        </script>
        </html>
```

## 39.3.4 요소 노드의 텍스트 노드 탐색

- 요소 노드의 텍스트 노드는 요소 노드의 자식 노드다. 따라서 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다. firstChild 프로퍼티는 첫 번째 자식 노드를 반환한다. firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.

```
<!DOCTYPE html>
<html>
<body>
    <div id='foo'>Hello</div>
    <script>
    // 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.
    console.log(document.getElementById('foo').firstChild); // #text
    </script>
    </body>
    </html>
```

## 39.3.5 부모 노드 탐색

- 부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용한다.
- 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없다.

```
<!DOCTYPE html>
<html>
 <body>
   <ul id="fruits">
     <li class="apple">Apple</li>
     <li class="banana">Banana</li>
     <li class="orange">ORangeㅎ</li>
   </ul>
 </body>
 <script>
   // 노드 탐색의 기점이 되는 .banana 요소 노드를 취득한다
   const $banana = document.querySelector('.banana');

   // .banana 요소 노드의 부모 노드를 탐색한다
   console.log('$banana.parentNode'); //ul#fruits
 </script>
</html>
```

## 39.3.6 형제 노드 탐색

- 부모 노드가 같은 형제 노드를 탐색하려면 다음과 같은 노드 탐색 프로퍼티를 사용한다.
- 단. 어트리뷰트 노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환되지 않는다.
- 텍스트 노드 또는 요소 노드만 반환한다.

  <img width="597" alt="스크린샷 2022-12-18 오전 12 01 49" src="https://user-images.githubusercontent.com/78072931/208248169-b9e29a99-bc8b-4291-b56a-b9fa641e05ff.png">

```
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">ORangeㅎ</li>
    </ul>
  </body>
  <script>
    // 노드 탐색의 기점이 되는 fruits 요소 노드를 취득한다
    const $fruits = document.getElementById('fruits');

    // #fruits 요소의 첫 번째 자식 노드를 탐색
    // firstChild 프로퍼티는 요소 노드 뿐만 아니라 텍스트 노드를 반환할 수도 있다
    const { firstChild } = $fruits;
    console.log(firstChild); // #text

    // #fruits 요소의 첫 번째 자식의 다음 형제 노드 노드를 탐색
    // nextSibling 프로퍼티는 요소 노드 뿐만 아니라 텍스트 노드를 반환할 수도 있다
    const { nextSiblig } = firstChild;
    console.log(nextSibling); // li.apple

    // li.apple 요소의 이전 형제 노드를 탐색
    // previousSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
    const { previousSibling } = nextSibling;
    console.log(previousSibling); // #text

    // #fruits 요소의 첫 번째 자식 요소 노드를 탐색
    // firstElementChild 프로퍼티는 요소 노드만 반환한다.
    const { firstElementChild } = $fruits;
    console.log(firstElementChild); // li.apple

    // #fruits 요소의 첫 번째 자식 요소 노드의 다음 형제 노드를 탐색
    // nextElementSibling 프로퍼티는 요소 노드만 반환한다.
    const { nextElementSibling } = firstElementChild;
    console.log(nextElementSibling); // li.banana

    // li.banana 요소의 의전 형제 요소 노드를 탐색한다.
    // previousElementSibling 프로퍼티는 요소 노드만 반환한다.
    const { previousElementSibling } = nextElementSibling;
    console.log(previousElementSibling); // li.apple


  </script>
</html>
```

## 39.4 노드 정보 취득

노드 정보 프로퍼티 : 노드 객체 정보 취득
프로퍼티|설명
-|-
Node.prototype.nodeType|노드 객체의 종류, 노드 타입 나타내는 상수 반환
_|노드 타입 상수는 Node에 정의
_|Node.ELEMENT*NODE : 요소 노드 타입, 상수 1
*|Node.TEXT*NODE : 텍스트노드 타입, 상수 3
*|Node.DOCUMENT*NODE : 문서 노드 타입, 상수 9
Node.prototype.nodeName|노드의 이름 문자열로 반환
*|요소 노드 : 대문자로 태그 이름("UL", "LI"등) 반환
_|텍스트 노드 : 문자열 "#text" 반환
_|문서 노드 : 문자열 "#document" 반환

```html
<!DOCTYPE html>
<html>
    <body>
        <div id="foo">Hello</div>
    </body>
    <script>
        // 문서 노드 정보 취득
        console.log(document.nodeType); // 9
        console.log(document.nodeName); // #document
        // 요소 노드 정보 취득
        const $foo = document.getElementById('foo');
        console.log($foo.nodeType); // 1
        console.log($foo.nodeName); // DIV
        // 텍스트 노드 정보 취득
        const $textNode = $foo.firstChild;
        console.log($textNode.nodeType); // 3
        console.log($textNode.nodeName); // #text
    < /script>
</html>
```

## 39.5 요소 노드의 텍스트조작

### 39.5.1 nodeValue

노드 탐색, 노드 정보 프로퍼티 : 읽기 전용
Node.prototype.nodeValue 프로퍼티 :

- setter, getter
- 참조, 할당 모두 가능
- 노드 객체 값 반환

노드 객체의 값 :

- 텍스트 노드 -> 텍스트
- 문서 노드, 요소 노드 -> null

```js
// 위동..
// 문서 노드 nodeValue 프로퍼티
console.log(document.nodeValue); // null
// 요소 노드 nodeValue 프로퍼티
const $foo = document.getElementById("foo");
console.log($foo.nodeValue); // null
// 텍스트 노드 nodeValue 프로퍼티
const $textNode = $foo.firstChild;
console.log($textNode.nodeValue); // Hello
```

텍스트 노드 -> 텍스트
텍스트 노드가 아닌 노드 -> null
텍스트 노드 값 할당 -> 텍스트 변경

1. 요소 노드 취득 -> 노드의 텍스트 노드 탐색 -> 자식노드 firstChild로 탐색
2. nodevalue로 텍스트 변경

```js
// 위동..
// 1. #foo 요소 노드의 자식 노드인 텍스트 노드 취득
const $textNode = document.getElementById("foo").firstChild;
// 2. nodeValue로 텍스트 변경
$textNode.nodeValue = "World";
console.log($textNode.nodeValue); // World
```

### 39.5.2 textcontent

- setter와 getter 존재
- 텍스트와 모든 자손 노드의 텍스트를 모두 취득, 변경
- 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트 모두 반환
- 모든 노드들의 텍스트 모두 반환(HTML 마크업 무시)

```js
// 위동..
// #foo 요소 노드의 텍스트 모두 취득(HTML 마크업은 무시)
console.log(document.getElementById("foo").textcontent); // Hello world!
```

### 그림39-13 textcontent 프로퍼티에 의한 텍스트 취득

div의 "Hello"와 span의 "world!" 모두 획득

nodevalue :

- 텍스트 취득
- 텍스트 노드가 아닌 노드는 null
- 코드가 더 복잡

```js
// #foo 텍스트 노드X
console.log(document.getElementById("foo").nodeValue); // null
// #foo 자식 노드 텍스트 노드 값 취득
console.log(document.getElementById("foo").firstChild.nodeValue); // Hello
// span 자식 노드 텍스트 노드 값 취득
console.log(document.getElementById("foo").lastChild.firstChild.nodeValue);
// world!
```

자식 요소 노드 없고, 텍스트만 존재하면 동일 결과

- -> textContent가 더 간단

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    const $foo = document.getElementById("foo");
    // 텍스트만 존재 같은 결과를 반환
    console.log($foo.textContent === $foo.firstChild.nodeValue); // true
  </script>
</html>
```

textContent에 문자열 할당

- -> 모든 자식 노드 제거 && 문자열 텍스트로 추가
- HTML 마크업이 포함 문자열 -> HTML 마크업 파싱X, 텍스트로

```js
// 위동 ..
// #foo 모든 자식 노드 제거, 할당한 문자열 텍스트로 추가
// HTML 마크업 파싱X
document.getElementById('foo').textcontent='Hi<span>there!</span>' ;
</script>
</html>
```

### 그림39-15 textcontent 프로퍼티에 의한 콘텐츠 변경

Hi <span>there!</span> 파싱되지 않고 화면에 그대로 출력

innerText(textContent와 유사) :

- 사용 비권고
- CSS에 의존적
- ex> visibility:hidden지정 요소 텍스트 반환X
- CSS 고려해야하므로 textContent보다 느림

## 39.6 DOM 조작

DOM 조작(DOM maniputation) :

- 새 노드 생성, DOM에 추가 하거나 기존 노드 삭제, 교체
- DOM에 새로운 노드가 추가, 삭제되면 -> 리플로우, 리페인트 발생(성능 영향)
  (38.7절 "리플로우와 리페인트" 자세히)
- 복잡한 콘텐츠 다루는 DOM 조작은 성능 최적화 위해 주의해서 다루어야함

### 39.6.1 innerHTML

Element.prototype.innerHTML

- setter, getter 모두 존재
- 요소 노드의 HTML 마크업 취득, 변경
- 콘텐츠 영역(시작 태그와 종료 태그 사이) 내 포함된 모든 HTML 마크업 문자열로 반환

```js
// #foo 콘텐츠 영역 내의 HTML 마크업 문자열로 취득
console.log(document.getElementById("foo").innerHTML);
// "Hello <span>world!</span>"
```

textcontent

- HTML 마크업 무시, 텍스트만 반환
  innerHTML
- HTML 마크업 포함된 문자열 그대로 반환
- 문자열 할당시
  모든 자식 노드 제거
  || 할당한 문자 자식 노드로 DOM에 반영 (HTML 마크업 파싱)

```js
// HTML 마크업 파싱되 자식노드로 DOM에 반영
document.getElementById("foo").innerHTML = "Hi<span>there!</span>";
```

### 그림 39-17 innerHTML 프로퍼티에 의한 DOM 조작

div->"Hi", div->span에 "there!"

HTML마크업 문자열로 간단히 DOM 조작 가능

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");
    // 노드 추가
    $fruits.innerHTML += '<li class="banana">Banana</li>';
    // 노드 교체
    $fruits.innerHTML = '<li class="orange">Orange</li>';
    // 노드 삭제
    $fruits.innerHTML = " ";
  </script>
</html>
```

innerHTML 할당한 HTML 마크업 문자열

- 렌더링 엔진에 의해 파싱, 요소 노드의 자식으로 DOM 반영
- 사용자로부터 입력받은 데이터를 그대로 할당하면
- -> 사이트 스크립팅 공격(XSS: Cross-Site Scripting Attacks)에 취약
- 악성 코드 포함되어 있다면 파싱 과정중 그대로 실행될 가능성 있기 때문

```js
// 위동 ..
// innerHTML로 스크립트 태그 삽입해 js 실행
// (HTML5) innerHTML로 삽입된 js 실행X
document.getElementById("foo").innerHTML =
  "<script>alert(document.cookie)</script>";
```

(HTML5) innerHTML로 삽입된 js 실행X
-> script 없이도 XSS 가능(모던 브라우저에서도 동작)

```js
// 에러 이벤트 강제로 발생 -> js 코드 실행
document.getElementById("foo").innerHTML =
  '<img src="x" onerror="alert(document.cookie)">';
```

innerHTML :

- 구현 간단, 직관적
- XSS에 취약 (단점 1)

> HTML 새니티제이션(HTML sanitization)
>
> > 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 XSS 예방위해
> > 잠재적 위험 제거하는 기능
> > 직접 구현 가능, DOMPruify 라이브러리 사용 권장
> > DOMPurify는 위험 새니티제이션(살균) -> 잠재적 위험 제거

```js
DOMPurify.sanitize('<img src="x" onerror="alert(document.cookie)">');
// => <img src="x">
```

> > DOMPurify(2014년 2월부터 제공) 어느 정도 안정성 보장된 새니티제이션 라이브러리

innerHTML 단점 2

- HTML 마크업 문자열 할당하면 모든 자식 노드 제거
- 할당한 문자열 파싱하여 DOM 변경

```js
const $fruits = document.getElementById("fruits");
// 노드 추가
$fruits.innerHTML += '<li class="banana">Banana</li>';
```

예상 동작

- li.apple은 변경 없어 다시 생성X
- li.banana 요소 노드만 생성 자식 요소로 추가

실제 동작

- 모든 자식 노드(li.apple) 제거
- 새로운 요소로 apple, banana 생성하여 자식 요소로 추가

```js
$fruits.innerHTML = $fruits.innerHTML + '<li class="banana">Banana</li>';
// '<li class="apple">Apple</li>' + '<li class="banana">Banana</li>'

// -> 축약 표현
$fruits.innerHTML += '<li class="banana">Banana</li>';
```

- 자식 노드 모두 제거
- 다시 만들고 추가 노드 까지 생성하여 DOM 반영(비효율)

innerHTML 단점3

- 새 요소 삽입될 위치 지정 불가

```html
<ul id="fruits">
  <li class="apple">Apple</li>
  <li class="orange">Orange</li>
</ul>
```

apple과 orange 사이 새 요소 삽입 불가

### innerHTML 정리

- 복잡하지 않은 요소 새롭게 추가시 유용
- 위치 지정 필요시 사용X

## 39.6 DOM 조작

- DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다. DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다. 따라서 복잡한 콘텐츠를 다루는 DOM 조작은 성능 최적화를 위해 주의해서 다루어야 한다.

## 39.6.1 innerHTML

- Element.prototype.innerHTML 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTMl 마크업을 취득하거나 변경한다. 요소 노드의 innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.

```
<!DOCTYPE html>
<html>
    <body>
        <div id='foo'> Hello <span> world! </span></div>
    </body>
    <script>
    // foo 요소의 콘텐츠 영역 내의 html 마크업을 문자열로 취득한다.
    console.log(document.getElementById('foo').innerHTML);
    // "Hello <span> world! </span>"
    </script>
</html>
```

- textContent 프로퍼티를 참조하면 HTML 마크업을 무시하고 텍스트만 반환하지만 innerHTML 프로퍼티는 HTML 마크업이 포함된 문자열을 그대포 반환한다.
- 요소 노드의 innerHTMl 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTMl 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.

```
<!DOCTYPE html>
<html>
    <body>
        <div id='foo'> Hello <span> world! </span></div>
    </body>
    <script>
    // HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.
    document.getElemnetById('foo').innerHTML = 'Hi <span> there!</span>';
    </script>
</html>
```

```
<!DOCTYPE html>
<html>
    <body>
       <ul id ='fruits'>
        <li class='apple'> Apple </li>
        </ul>
        </body>
    <script>
        const $fruits = document.getElementById('fruits');

        // 노드 추가
        $fruits.innerHTML += '<li class="banana">Banana</li>';

        // 노드 교체
        $fruits.innerHTML = '<li class="orange">Orange</li>';

        // 노드 삭제
        $fruits.innerHTML = '';

    </script>
</html>
```

- 요소 노드의 innerHTML 프로퍼티에 할당한 HTML 마크업 문자열은 렌더링 엔진에 의해 파싱되어 요소 노드의 자식으로 DOM에 반영된다. 이때 사용자로부터 입력받은 데이터를 그대로 innerHTML 프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격에 취약하므로 위험하다.

```
<!DOCTYPE html>
<html>
    <body>
    <div id='foo'> Hello</div>
    </body>
    <script>
    // innerHTML 프로퍼티로 스크립트 태그를 삽입하여 자바스크립트가 실행되도록 한다.
    // HTML5는 innerHTML 프로퍼티로 삽입된 script 요소 내의 자바스크립트 코드를 실행하지 않는다.
    document.getElementById('foo').innerHTML = '<script>alert(document.cookie)</script>';
</script>
</html>
```

- HTMl 새니티제이션: HTML 새니티제이션은 자용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠재적 위험을 제거하는 기능을 말한다. 새니티제이션 함수를 직접 구현할 수도 있겠지만 DOMPurify 라이브러리를 사용하는 것을 권장한다.
  DOMPurify는 다음과 같이 잠재적 위험을 내포한 HTML 마크업을 새니티제이션(살균)하여 잠재적 위험을 제거한다.

  - innerHTML 프로퍼티의 또 다른 단점은 요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM을 변경한다는 것이다.

  ```
  // 기존에 있던 fruits 요소에 자식 요소를 추가하는 코드
  $fruits.innerHTML = $fruits.innerHTML + '<li class = "banana">Banana</li>';
  ```

  - innerHTML 프로퍼티를 사용하면 삽입 위치를 지정할 수 없다. innerHTML 프로퍼티는 복잡하지 않은 요소를 새롭게 추가할 때 유용하지만 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입해야 할 때는 사용하지 않는 것이 좋다.

## 39.6.2 insertAdjacentHTML 메서드

- Element.prototype.insertAdjacentHTML(position, DOMString) 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.
- position은 총 4가지다 : 'beforebegin', 'afterbegin', 'beforeend', 'afterend'
- innerHTML 프로퍼티보다 효율적이고 빠르다.
- 크로스 사이트 스크립팅 공격에 취약한건 동일하다.

```
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="1">1번</li>
      <li id="3">3번</li>
    </ul>
    <script>
      document.getElementById('1').insertAdjacentHTML("afterend",'<li id="2">2번</li>');
    </script>
  </body>
</html>
```

## 39.6.3 노드 생성과 추가

- 지금까지 살펴본 innerHTML 프로퍼티와 insertADjacentHTML 메서드는 HTML 마크업 문자열을 파싱하여 노드를 생성하고 DOM에 반영한다. DOM은 노드를 직접 생성, 삽입, 삭제, 치환하는 메서드도 제공한다.
  - 요소 노드 생성: document.prototype.createElement(tagName)
  - 텍스트 노드 생성: document.prototype.createTextNode(text)
  - 마지막 자식 노드로 추가: Node.prototype.appendChild(childNode)

```
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="1">1번</li>
      <li id="2">2번</li>
    </ul>
    <script>
      const table =document.querySelector('ul');
      const liNode = document.createElement('li'); // 요소 노드 생성
      const textNode = document.createTextNode('3번'); // 텍스트 노드 생성
      liNode.appendChild(textNode); // 요소 노드에 자식으로 텍스트 노드 추가
      table.appendChild(liNode); // table에 자식으로 요소 노드 추가
    </script>
  </body>
</html>
```

- 요소 노드 생성: Document.prototype.createElement(tagName) 메서드는 요소 노드를 생성하여 반환한다. createElement 메서드의 매개변수 tagName에는 태그 이름을 나타내는 문자열을 인수로 전달한다.
- createElement 메서드로 생성한 요소 노드는 기존 DOM에 추가되지 않고 홀로 존재하는 상태다.
- 이후에 생성된 요소 노드를 DOM에 추가하는 처리가 별도로 필요하다.
- createElement 메서드로 생성한 요소 노드는 아무런 자식 노드를 가지고 있지 않다. 따라서 요소 노드의 자식 노드인 텍스트 노드도 없는 상태이다.

```
// 요소 노드 생성
const $li = document.createElement('li');
// 생성된 요소 노드는 아무런 자식 노드가 없다
console.log($li.childNodes); // NOdeList []
```

## 39.6.4 복수의 노드 생성과 추가

```
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="1">1번</li>
      <li id="2">2번</li>
    </ul>
    <script>
      const table =document.querySelector('ul');
      ['3번','4번','5번'].forEach(text=>{
        const newNode=document.createElement('li');
        newNode.textContent=text;
        table.appendChild(newNode); // 리플로우,리페인트 3번이나 발생
      });
    </script>
  </body>
</html>
```

- 컨테이너 요소를 미리 만들어놓고, 거기에 새롭게 생성한 노드를 추가하고 마지막에 컨테이너 요소를 추가시키면 리플로우나, 리페인트는 한번만 발생하게 할 수 있다.

```
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="1">1번</li>
      <li id="2">2번</li>
    </ul>
    <script>
      const table =document.querySelector('ul');
      const container = document.createElement('div'); // 컨테이너 요소 생성

      ['3번','4번','5번'].forEach(text=>{
        const newNode=document.createElement('li');
        newNode.textContent=text;
        container.appendChild(newNode); // 컨테이너 요소에 추가
      });
      table.appendChild(container); // 리플로우,리페인트 1번만 발생
    </script>
  </body>
</html>
```

- li요소만 추가되는 것이 아니고, 이것들을 감싸고 있는 div요소도 같이 추가되는 문제가 생긴다.
- 컨테이너 요소를 만들 때 DocumentFragment 노드를 만들면 해결이 가능하다.
- DocumentFragment를 DOM에 추가하면 자신은 제거되고 자식 노드만 DOM에 추가된다.
- Document.prototype.createDocumentFragment 메서드로 생성한다.

```
const container = document.createElement('div'); // 일반 노드 만들기 x
const container = document.createDocumentFragment(); // DocumentFragment로 만들기
```

## 39.6.5 노드 삽입

- 마지막 노드로 추가: Node.prototype.appendChild 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가한다. 이때 노드를 추가할 위치를 지정할 수 없고 언제나 마지막 자식 노드로 추가한다.
- 지정한 위치에 노드 삽입: Node.prototype.insertBefore(newNode, childNode) 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다.
- 두번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드이어야 하고, 그렇지 않으면 에러 발생
- 두번째 인수가 null이면 마지막 노드에 추가가 된다.

## 39.6.6 노드 이동

- DOM에 이미 존재하는 노드를 appendChild,insertBefore 메서드로 DOM에 추가하면 원래 있던 위치의 노드는 제거되고 새로운 위치로 노드를 추가한다.(노드가 이동)

```
<!DOCTYPE html>
<html>
  <body>
    <ul id = "fruits">
      <li>Apple</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');
      const $apple = $fruits.firstElementChild;

      // $apple 요소를 얕은 복사하여 사본을 생성. 텍스트 노드가 없는 사본이 생성된다.
      const $shallowClone = $apple.cloneNode();

      // 사본 요소 노드에 텍스트 추가
      $shallowClone.textContent = 'Banana';

      // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
      $fruits.appendChild($shallowClone);

      // #fruits 요소를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성
      const $deepClone = $fruits.cloneNode(true);

      // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
      $fruits.appendChild($deepClone);
      </script>
    </html>
```

## 39.6.8 노드 교체

- Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.

```
// 기존 노드와 교체할 요소 노드를 생성
const $newChild = document.createElement('li');
$newChild.textContent = 'Banana';

// #fruits 요소 노드의 첫 번째 자식 요소 노드를 $newChild 요소 노드로 교체
$fruits.replaceChild($newChild, $fruits.firstElementchild);
```

## 39.6.9 노드 삭제

- Node.prototype.removeChild(child) 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다. 인수로 전달한 노드는 removechild 메서드를 호출한 노드의 자식 노드이어야 한다.

```
// #fruits 오소 노드의 마지막 요소를 DOM에서 삭제
$fruits.removeChild($fruits.lastElementChild);
```

# 39.7 어트리뷰트

## 39.7.1 어트리뷰트 노드와 attributes 프로퍼티

HTML 요소(HTML 문서의 구성 요소) :

- 여러 개의 어트리뷰트(attribute, 속성) 가질 수 있음

HTML 어트리뷰트

- HTML 요소의 동작 제어 위한 부가정보 제공
- HTML 요소의 시작 태그(start/opening tag)에 어트리뷰트 이름="어트리뷰트 값" 형식으로 정의

```html
<input id="user" type="text" value="ungmo2" />
```

모든 HTML 요소에서 공통적으로 사용 :

- 글로벌 어트리뷰트(id, class, style, title , lang, tabindex, draggable, hidden 등)
- 이벤트 핸들러 어트리뷰트(onclick, onchange, onfocus, onblur, oninput, onkeypress, onkeydown, onkeyup, onmouseover,
  onsubmit, onload 등)
- id, class, style 어트리뷰트

특정 HTML 요소에만 한정적 사용 가능 어트리뷰트

- type, value, checked 어트리뷰트 -> input 요소에만

HTML 문서가 파싱될 때 HTML 요소의 어트리뷰트(= HTML 어트리뷰트) :

- 어트리뷰트 노드로 변환, 요소 노드와 연결
- HTML 어트리뷰트당 하나의 어트리뷰트 노드 생성
- ex> input 요소 3개 어트리뷰트 -> 3개 어트리뷰트 노드 생성
- 모든 어트리뷰트 노드의 참조는 NamedNodeMap 객체(유사 배열 객체 || 이터러블)에 담겨
  -> 요소 노드의 attributes 프로퍼티에 저장

```js
<input id="user" type="text" value="ungmo2">
  _
</input>
// Chrome > Elements > 요소 선택시 Properties 를 살펴보면
// attributes(NamedNodeMap)에 id, type, value 있음
// 3개 프로퍼티가 세개의 어트리뷰트 노드로
```

요소 노드 모든 어트리뷰트 노드 :

- 요소 노드의 Element.prototype.attributes 프로퍼티로 취득 가능
- attributes 프로퍼티는 getter만 존재(읽기 전용 접근자 프로퍼티)
- 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체 반환

```html
<!DOCTYPE html>
<html>
  <body>
    <input id="user" type="text" value="ungmo2" />
    <script>
      // 요소 노드의 attribute 프로퍼티는 요소 노드의 도든 어트리뷰트 노드의 참조가 담긴
      // NamedNodeMap 객체를 반환한다.
      const { attributes } = document.getElementById("user");
      console.log(attributes);
      // NamedNodeMap {0:id, 1:type, 2:value, id: id, type: type, value: value, length: 3}
      // 어트리뷰트 값 취득
      console.log(attributes.id.value); // user
      console.log(attributes.type.value); // text
      console.log(attributes.value.value); // ungmo2
    </script>
  </body>
</html>
```

## 39.7.2 HTML 어트리뷰트 조작

attributes 프로퍼티 :

- getter만 존재 읽기 전용 접근자 프로퍼티
- HTML 어트리뷰트 값을 취득만 가능(변경불가)
- attributes.id.value 처럼 attributes 프로퍼티 통해야만 HTML 어트리뷰트 값 취득가능

Element.prototype.getAttribute/setAttribute 메서드

- attributes 프로퍼티 통하지 않고 HTML 어트리뷰트 값 취득 및 변경 가능

Element.prototype.getAttribute(attributeName) 메서드 : HTML 어트리뷰트 값 참조
Element.prototype.setAttribute(attributeName, attributeValue) 메서드 : HTML 어트리뷰트 값 변경

```js
// 위동..
const $input = document.getElementById("user");
// value 어트리뷰트 값을 취득
const inputvalue = $input.getAttribute("value");
console.log(inputvalue); // ungmo2
// value 어트리뷰트 값을 변경
$input.setAttribute("value", "foo");
console.log($input.getAttribute("value")); //foo
```

Element.prototype.hasAttribute(attributeName) 메서드 : 특정 HTML 어트리뷰트 존재 확인
Element.prototype.removeAttribute(attributeName) 메서드 : 특정 HTML 어트리뷰트 삭제

```js
const $input = document.getElementById("user");
// value 어트리뷰트의 존재 확인
if ($input.hasAttribute("value")) {
  // value 어트리뷰트 삭제
  $input.removeAttribute("value");
}
// value 어트리뷰트 삭제 확인
console.log($input.hasAttribute("value")); // false
```

## 39.7.3 HTML 어트리뷰트 vs. DOM 프로퍼티

요소 노드 객체에 HTML 어트리뷰트에 대응하는 프로퍼티(이하 DOM 프로퍼티) 존재
DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가짐

ex>

```html
<input id="user" type="text" value="ungmo2" />
```

요소 파싱되 생성된 요소 노드 객체에

- id, type, value 어트리뷰트에 대응하는 id, type, value 프로퍼티 존재
- DOM 프로퍼티들은 HTML 어트리뷰트의 값을 초기값으로 가짐

```html
<input id="user" type="text" value="ungmo2" />
<script>
  // 개발자 작업도구 > 요소 > 프로퍼티 확인시
  // id, type, value에 대응하는 프로퍼티 존재
</script>
```

그림39-32 HTML 어트리뷰트에 대응하는 요소 노드의 프로퍼티

### DOM 프로퍼티 :

- setter와 getter 모두 존재하는 접근자 프로퍼티
- 참조와 변경 가능

```js
// 위동..
const $input = document.getElementById("user");
// 요소 노드의 value 프로퍼티 값 변경
$input.value = "foo";
// 요소 노드의 value 프로퍼티 값 참조
console.log($input.value); // foo
```

HTML 어트리뷰트는 DOM에서 중복 관리(->X)

1. 요소 노드의 attributes 프로퍼티에서 관리하는 어트리뷰트 노드
2. HTML 어트리뷰트에 대응하는 요소 노드의 프로퍼티(DOM 프로퍼티)

HTML 어트리뷰트 역할 :

- HTML 요소의 초기 상태 지정
- HTML 어트리뷰트 값은 HTML 요소의 초기 상태를 의미하며 이는 변하지 않음

```html
<input id="user" type="text" value="ungmo2" />
```

- 요소의 value 어트리뷰트는 input 요소가 렌더링될 때 입력 필드에 표시할 초기값을 지정
- input 요소가 렌더링되면 입력 필드에 초기값으로 지정한 value 어트리뷰트 값 "ungmo2"가 표시
- input 요소의 value 어트리뷰트는 어트리뷰트 노드 변환
- 요소 노드의 attributes 프로퍼티에 저장
- value 어트리뷰트의 값은 요소 노드의 value 프로퍼티 할당
- input 요소의 요소 노드가 생성
- 첫 렌더링이 끝난 시점까지
  어트리뷰트 노드의 어트리뷰트 값과 요소 노드의 value 프로퍼티에 할당 값 HTML 어트리뷰트 값과 동일

```js
const $input = document.getElementById("user");
// attributes 프로퍼티에 저장된 value 어트리뷰트 값
console.log($input.getAttribute("value")); // ungmo2
// 요소 노드의 value 프로퍼티에 저장된 value 어트리뷰트 값
console.log($input.value); // ungmo2
```

첫 렌더링 이후

- 사용자가 input 요소에 입력시
- 요소 노드는 상태를 가지고 있음
- input 요소 노드는 사용자가 입력 필드에 입력한 값을 상태로 가짐
- checkbox 요소 노드는 사용자가 입력 필드에 입력한 체크 여부 상태 가짐

ex> input 요소에 "foo" 입력시

- input 요소 노드는 최신 상태("foo")와 초기 상태("ungmo2") 관리
- 요소 노드는 2개의 상태 관리
- 초기 상태는 어트리뷰트 노드가 관리
- 최신 상태는 DOM 프로퍼티가 관리
  > 초기 상태로 웹페이지 처음 표시때, 새로고침때 초기 상태 표시

### 어트리뷰트 노드

- 요소의 초기 상태는 어트리뷰트 노드에서 관리
- 사용자의 입력에 의해 변경 되지 않고 유지
- getAttribute/setAttribut 메서드 : 초기 상태값 취득, 변경

getAttribut 메서드 : 어트리뷰트 노드에서 관리하는 HTML 요소에 지정한 어트리뷰트 값(초기 상태 값)

```js
// attributes 프로퍼티에 저장된 value 어트리뷰트 값 취득, 결과 언제나 동일
document.getElementById("user").getAttribute("value"); // ungmo2
```

setAttribute 메서드 : 어트리뷰트 노드에서 관리하는 HTML 요소에 지정한 어트리뷰트 값(초기 상태 값 변경)

```js
// HTML 요소에 지정한 어트리뷰트 값, 초기 상태 값 변경
document.getElementById("user").setAttribute("value", "foo");
```

### DOM 프로퍼티

- 최신 상태 : 사용자 입력, 요소 노드의 DOM 프로퍼티가 관리
- 언제나 최신 상태 유지(동적변경, 최신 상태 유지)

```js
const $input = document.getElementById("user");
// 사용자가 input 요소 입력할 때, input 요소 노드의 value 프로퍼티 값, 최신 상태 값 취득
// value 프로퍼티 값은 입력에 의해 동적변경
$input.oninput = () => {
  console.log("value프로퍼티값", $input.value);
};
// getAttribute 메서드로 취득한 HTML 어트리뷰트 값(초기 상태 값) 유지
console.log("value 어트리뷰트 값", $input.getAttribute("value"));
```

DOM 프로퍼티에 값 할당 : HTML 요소의 최신 상태 값 변경
-> 사용자가 상태를 변경하는것과 같음

```js
const $input = document.getElementById("user");
// DOM 프로퍼티에 값을 할당하여 HTML 요소의 최신 상태 변경
$input.value = "foo";
console.log($input.value); // foo
// getAttribute 메서드로 취득한 HTML 어트리뷰트 값(초기 상태 값) 유지
console.log($input.getAttribute("value")); // ungmo2
```

HTML 어트리뷰트 : 요소의 초기 상태 값
DOM 프로퍼티 : 최신 상태 관리

> 모든 DOM 프로퍼티가 최신 상태 관리 X

ex> input 요소 : value 프로퍼티가 관리
checkbox 요소 : checked 프로퍼티가 관리
-> id 어트리뷰트에 대응하는 id 프로퍼티는 입력과 관계 X(항상 동일)
-> id 어트리뷰트 값이 변하면 id 프로퍼티 값도 변하고 그 반대도 마찬가지

```js
const $input = document.getElementById("user");
// id 어트리뷰트와 id 프로퍼티는 항상동일 값으로 연동
$input.id = "foo";
console.log($input.id); // foo
console.log($input.getAttribute("id")); // foo
```

상태 변화와 관계있는 DOM 프로퍼티만 최신 상태 값 관리
그 외는 어트리뷰트와 DOM 프로퍼티 항상 동일값 연동

### HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계

- HTML 어트리뷰트는 HTML 어트리뷰트 이름과 동일한 DOM 프로퍼티와 1:1로 대응
- 아래는 언제나 1:1 아님
- 아래는 HTML 어트리뷰트 이름과 DOM 프로퍼티 키 일치X

- id 어트리뷰트 == id 프로퍼티 1:1 대응, 동일값 연동
- input 요소의 value 어트리뷰트는 value 프로퍼티와 1:1 대응
  -> value 어트리뷰트는 초기 상태 value 프로퍼티는 최신 상태
- class 어트리뷰트는 className, classList 프로퍼티와 대응(39.8.2 "클래스 조작" 자세히)
- for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응
- td 요소의 colspan 어트리뷰트는 대응 프로퍼티 없음
- textcontent 프로퍼티는 대응 어트리뷰트 없음
- 어트리뷰트 이름은 대소문자 구별X, 대응 프로퍼티 키는 카멜 케이스 (maxlength -> maxLength)

### DOM 프로퍼티 값의 타입

getAttribute 메서드 :

- 취득한 어트리뷰트 값은 언제나 문자열
- DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수 있음
- ex> checkbox 요소의 checked 어트리뷰트 값 문자열, 프로퍼티 값 불리언

```html
<body>
  <input type="checkbox" checked />
  <script>
    const $checkbox = document.querySelector("input[type=checkbox]");
    // getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열
    console.log($checkbox.getAttribute("checked")); // ''
    // DOM 프로퍼티로 취득한 최신 상태 값은 문자열 아닐 수 있음
    console.log($checkbox.checked); // true
  </script>
</body>
```

## 39.7.4 data 어트리뷰트 dataset 프로퍼티

data 어트리뷰트와 dataset 프로퍼티를 사용시

- HTML 요소에 정의한 사용자 정의 어트리뷰트와 js간 데이터 교환 가능
- data 어트리뷰트는 data-user-id, data-role 처럼 data- 접두사 붙여 사용

```html
<body>
  <ul class="users">
    <li id="1" data-user-id="7621" data-role="admin">Lee</li>
    <li id="2" data-user-id="9524" data-role="subscriber">Kim</li>
  </ul>
</body>
```

- data 어트리뷰트의 값은 HTMLElement.dataset 프로퍼티로 취득 가능
- dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체 반환
- DOMStringMap 객체는 data 어트리뷰트의 data- 접두사 다음에 붙인 임의의
  이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있음
  -> 이 프로퍼티로 data 어트리뷰트 값 취득, 변경 가능

```js
// 위동..
const users = [...document.querySelector(".users").children];
// user-id가 '7621'인 요소 노드 취득
const user = users.find((user) => user.dataset.userid === "7621");
// user-id가 '7621'인 요소 노드에서 data-role값 취득
console.log(user.dataset.role); // "admin"
// user-id가 '7621'인 요소 노드의 data-role을 변경
user.dataset.role = "subscriber";
// dataset 프로파티는 DOMStringMap 객체 반환
console.log(user.dataset); // DOMStringMap {userid: "7621", role: "subscriber"}
```

data 어트리뷰트

- data- 접두사 다음에 존재하지 않는 이름을 키로 사용
- dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트 추가
- dataset 프로퍼티에 추가한 카멜케이스(fooBar)의 프로퍼티 키는
  data 어트리뷰트의 data- 접두사 다음에 케밥케이스(data-foo-bar)로 자동 변경, 추가

```js
// 위동..
const users = [...document.querySelector(".users").children];
// user-id '7621'요소 노드 취득
const user = users.find((user) => user.dataset.userid === "7621");
// user-id '7621'요소 노드에 새 data 어트리뷰트 추가
user.dataset.role2 = "admin";
console.log(user.dataset);
/*
DOMStringMap {userid: "7621", role2: "admin"}
-> <li id = "1" data-user-id="7621" data-role2="admin">Lee</li>
*/
```

# 39.8 스타일

## 39.8.1 인라인스타일 조작

HTMLElement.prototype.style 프로퍼티

- setter/getter 접근자 프로퍼티
- 요소 노드의 인라인 스타일(inline style) 취득, 추가, 변경

```html
<body>
  <div style="color: red">Hello World</div>
  <script>
    const $div = document.querySelector("div");

    // 인라인 스타일 취득
    console.log($div.style); // CSSStyleDeclaration { 0: "color", ... }

    // 인라인 스타일 변경
    $div.style.color = "blue";

    // 인라인 스타일 추가
    $div.style.width = "100px";
    $div.style.height = "100px";
    $div.style.backgroundcolor = "yellow";
  </script>
</body>
```

### 그림 39-34 style 프로퍼티에 의한 인라인 스타일 조작

- 크롬 개발자 도구 > Elements > Styles 에서 살펴보면 위 작성된 html과 다르게 스타일이 적용되어있음
- color : blue, width : 100px, height: 100px, background-color: yellow로

style 프로퍼티 참조시

- CSSStyleDeclaration 타입 객체 반환

CSSStyleDeclaration 객체

- 다양한 CSS 프로퍼티에 대응하는 프로퍼티 가짐
- 프로퍼티에 값 할당하면 해당 CSSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가, 변경
- CSS 프로퍼티는 케밥 케이스 따름
- 대응하는 CSSStyleDeclaration 객체의 프로퍼티는 카멜 케이스
- ex> CSS 프로퍼티 background-color 대응 CSSStyleDeclaration 객체 프로퍼티 backgroundcolor

```js
$div.style.backgroundcolor = "yellow";
// 케밥 케이스의 CSS 프로퍼티 그대로 사용 하려면 객체의 마침표 표기법 대신 대괄호 표기법 사용
$div.style["background-color"] = "yellow";
```

단위 지정 필요한 CSS 프로퍼티의 값은 반드시 단위 지정

- ex> px, em, % 등이 필요한 width에 단위 생략시 CSS 프로퍼티 적용X

```js
$div.style.width = "100px";
```

## 39.8.2 클래스 조작

.으로 시작하는 클래스 선택자 사용해 CSS class 미리 정의해
-> HTML 요소의 class 어트리뷰트 값 변경해 HTML 요소의 스타일 변경가능

HTML 요소의 class 어트리뷰트 조작 (39.7절 "어트리뷰트")

- class 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티 사용
- class 어트리뷰트에 대응하는 DOM 프로퍼티는 class가 아니라 className과 classList
- js에서 class는 예약어

### className

Element.prototype.className 프로퍼티

- setter/getter 접근자 프로퍼티
- HTML 요소의 class 어트리뷰트 값 취득, 변경
- 요소 노드의 className 프로퍼티를 참조시 class 어트리뷰트 값 문자열 반환
- 요소 노드의 className 프로퍼티에 문자열 할당시 할당한 문자열로 변경

```html
<head>
  <style>
    .box {
      width: 100px;
      height: 100px;
      background-color: antiquewhite;
    }
    .red {
      color: red;
    }
    .blue {
      color: blue;
    }
  </style>
</head>
<body>
  <div class="box red">Hello World</div>
  <script>
    const $box = document.querySelector(".box");

    // .box 요소의 class 어트리뷰트 값 취득
    console.log($box.className); // 'box red'

    // .box 요소의 class 어트리뷰트 값 중 'red' -> 'blue' 변경
    $box.className = $box.className.replace("red", "blue");
  </script>
</body>
```

className 프로퍼티

- 문자열 반환
- 공백으로 구분된 여러 개의 클래스 반환시 다루기 불편

## classList

Element.prototype.classList 프로퍼티 :

- class 어트리뷰트의 정보 담은DOMTokenList 객체 반환

```js
// 위동 ..
const $box = document.querySelector(".box");

// .box 요소의 class 어트리뷰트 정보를 담은 DOMTokenList 객체 취득
// DOMTokenList 객체는 HTMLCollection 같은 변화 실시간 반영하는 live 객체
console.log($box.classList);
// DOMTokenList(2) [length: 2, value: "box blue", 0:"box", 1:"blue"]
// .box 요소의 class 어트리뷰트 값 중 'red' -> 'blue' 변경
$box.classList.replace("red", "blue");
```

DOMTokenList :

- class 어트리뷰트들
- 유사배열 && 이터러블

### DomTokenList 제공 메서드

#### add(...className)

- 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가

```js
$box.classList.add("foo"); // -> class="box red foo"
$box.classList.add("bar", "baz"); // -> class="box red foo bar baz"
```

#### remove(...className)

- 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제
- 일치하는 클래스가 class 어트리뷰트에 없으면 에러 없이 무시

```js
$box.classList.remove("foo"); // -> class="box red bar baz"
$box.classList.remove("bar", "baz"); // -> class="box red"
$box.classList.remove("x"); // -> class="box red" // 무시
```

#### item(index)

- 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환
- index 0 : 첫 번째 클래스
- index 1 : 두 번째 클래스

```js
$box.classList.item(0); // -> "box"
$box.classList.item(1); // -> "red"
```

#### contains( classNaire)

- 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인

```js
$box.classList.contains("box"); // -> true
$box.classList.contains("blue"); // -> false
```

#### replace( oldClassName, newClassName)

- 첫 번째 인수로 전달한 문자열을 class 어트리뷰트에서 찾아 두 번째 인수로 전달한 문자열로 변경

```js
$box.classList.replace("red", "blue"); // -> class="box blue"
```

#### toggle(className[, force])

- 인수로 전달한 문자열과 일치하는 클래스가 존재하면 class 어트리뷰트에서 제거
- 존재하지 않으면 추가

```js
$box.classList.toggle("foo"); // -> class="box blue foo" // 없음, 추가
$box.classList.toggle("foo"); // -> class="box blue" // 있음, 제거
```

- 두 번째 인수로 조건식(불리언) 전달가능
- 첫 번째 인수로 전달한 문자열 true면 class 어트리뷰트에 강제 추가, false면 제거

```js
// class 어트리뷰트에서 강제로 'foo' 클래스를 추가
$box.classList.toggle("foo", true); // -> class=nbox blue foo"
// class 어트리뷰트에서 강제로 'foo' 클래스를 제거
$box.classList.toggle("foo", false); // -> class="box blue"
```

#### 그외 DOMTokenList 객체 제공 메서드

- forEach, entries, keys, values, supports

## 39.8.3 요소에 적용되어 있는 CSS 스타일 참조

style 프로퍼티 :

- 인라인 스타일만 반환
- 클래스를 적용한 스타일, 상속 통한 암묵적 스타일은 style 프로퍼티로 참조 불가

getComputedStyle(element[, pseudo]) 메서드 :

- window.getComputedStyle
- HTML 요소에 적용되어 있는 모든 CSS 스타일 참조
- 첫 번째 인수(element) : 요소 노드에 적용된 스타일 CSSStyleDeclaration 객체에 담아 반환
- 두 번째 인수(pseudo)로 :after, :before와 같은 의사 요소를 지정하는 문자열 전달가능
  (의사 요소 아닌 일반 요소의 경우 두 번째 인수 생략)

평가된 스타일(computed style)

- 요소 노드에 적용되어 있는 모든 스타일

모든 스타일

- 링크 스타일, 임베딩 스타일, 인라인 스타일
- 자바스크립트에서 적용한 스타일, 상속된 스타일, 기본(user agent) 스타일 등
- 모든 스타일이 조합되어 최종적으로 적용된 스타일

```html
<head>
  <style>
    body {
      color: red;
    }
    box {
      width: 100px;
      height: 50px;
      background-color: cornsilk;
      border: 1px solid black;
    }
  </style>
</head>
<body>
  <div class="box">Box</div>
  <script>
    const $box = document.querySelector(".box");

    // .box 요소에 적용된 모든 CSS 스타일을 담고 있는 CSSStyleDeclaration 객체 취득
    const computedStyle = window.getComputedStyle($box);
    console.log(computedStyle); // CSSStyleDeclaration

    // 임베딩 스타일
    console.log(computedStyle.width); // 100px
    console.log(computedStyle.height); // 50px
    console.log(computedStyle.backgroundcolor); // rgb(255, 248, 220)
    console.log(computedStyle.border); // 1px solid rgb(0, 0, 0)

    // 상속 스타일(body -> .box)
    console.log(computedStyle.color); // rgb(255, 0, 0)

    // 기본스타일
    console.log(computedStyle.display); // block
  </script>
</body>
```

2번째 메서드

```html
<head>
  <style>
    .box:before {
      content: "Hello";
    }
  </style>
</head>
<body>
  <div class="box">Box</div>
  <script>
    const $box = document.querySelector(".box");

    // 의사요소 :before의 스타일을 취득
    const computedStyle = window.getComputedStyle($box, ":before");
    console.log(computedStyle.content); // "Hello"
  </script>
</body>
```

# 39.9 DOM 표준

HTML과 DOM 표준 :

- W3G(World Wide Web Consortium), WHATWG(Web Hypertext Application Technology Working Group)
- 두 단체가 협력하며 공통 표준 만들어옴
- 두 단체가 서로 다른 결과물을 내놓음 (최근)
- 2018년 4월 부터 구글, 애플, MS, 모질라로 구성된 4개의 주류 브라우저 벤더사가 주도하는 WHATWG이
- 단일 표준 내놓기로 두 단체가 합의

| DOM Level | 표준 문서 URL                           |
| --------- | --------------------------------------- |
| 1         | https://vwvw.w3.org/TR/REC-DOM-Leve1-1  |
| 2         | https://vwvw.w3.org/TR/DOM-Level-2-Core |
| 3         | https://vwvw.w3.org/TR/DOM-Level-3-Core |
| 4         | https://dom.spec.whatwg.org             |
