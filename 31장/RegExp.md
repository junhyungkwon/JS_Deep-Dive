# 31장 RegExp

# 31.1 정규표현식이란?

정규 표현식(regular expression) : 패턴을 가진 문자열의 집합 표현 위한 형식 언어(formal language)

- 문자열 대상 패턴 매칭 기능 제공
- 패턴 매칭 기능 : 특정 패턴과 일치하는 문자열 검색, 추출, 치환 기능
- 반복문과 조건문 없이 패턴을 정의하고 재사용, 간단히 체크 가능
- 주석, 공백 사용 불가 가독성 나쁨

```js
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = "010-1234-5678";
// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;
// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); // -> false
```

# 31.2 정규 표현식의 생성

정규 표현식 객체(RegExp 객체)를 생성

- 정규 표현식 리터럴
- RegExp 생성자 함수

  /regexp/i
  맨앞 / : 시작
  regexp : 패턴
  맨뒤 /i : 종료

정규 표현식 리터럴은 패턴 + 플래그
RegExp 생성자 함수를 사용하여 RegExp 객체를 생성

```js
/**
 * pattern: 정규 표현식의 패턴
 * flags: 정규 표현식의 플래그(g, i , m, u, y)
 */
new RegExp(pattern[, flags ])
```

```js
// 정규 표현식 리터럴로
const target = "Is this all there is?";
// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색
const regexp = /is/i;
// test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여 매칭 결과를
// 불리언 값으로 반환한다.
regexp.test(target); // -> true

// 생성자로
const regexp2 = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');
regexp2.test(target); // -> true
```

# 31.3 RegExp 메서드

정규표현식을 사용하는 메서드
RegExp.prototype에 exec,test
String.prototype에 match, replace, search, split

몇개는 32장 "String" 자세히

## 31.3.1 RegExp.prototype.exec

패턴 검색 매칭 결과 배열반환, 없으면 null

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

g플래그 사용해도 첫번째것만 반환함

> g플래그 : 여러개 검색결과 반환

## 31.3.2 RegExp.prototype.test

패턴 검색 매칭 결과 불리언 반환

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); // -> true
```

## 31.3.3 String.prototype.match

String 빌트인 메서드, 결과 배열 반환

```js
const target = "Is this all there is?";
const regExp = /is/;
target.match(regExp);
// -> ["is", index: 5, input: "Is this a ll there is?", groups: undefined]

const regExp2 = /is/g; // g플래그, 모든 매칭 배열 반환
target.match(regExp2); // -> ["is", "is"]
```

# 31.4 플래그(옵셔널)

| 플래그 | 의미        | 설명                                                       |
| ------ | ----------- | ---------------------------------------------------------- |
| i      | Ignore case | 대소문자를 구별하지 않고 패턴을 검색                       |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색 계속                    |

- 순서와 상관없이, 동시 설정가능
- 플래그 없을때 -> 대소문자를 구별, 첫 번째 매칭한 대상만 검색

```js
const target = "Is this all there is?";
// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색한다.
target.match(/is/);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
target.match(/is/i);
// -> ["Is", index: 3, input: "Is this all there is?n, groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g);
// -> ["is", "is"]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi);
// -> ["Is", "is", -is"]
```

# 31.5 패턴

- 패턴과 플래그로 구성
- 패턴 : 규칙 설정, /로 열고 닫음, 문자열 따옴표 생략
- 플래그 : 검색 방식
- 메타문자 : 특별한 의미를 가지는 기호로 표현 가능

* 따옴표를 포함하면 따옴표까지 검색

## 31.5.1 문자열 검색

```js
const target = "Is this all there is?";
// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/;
// target과 정규 표현식이 매치하는지 테스트한다.
regExp.test(target); // -> true
// target과 정규 표현식의 매칭 결과를 구한다.
target.match(regExp);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]

// 'is'문자열과 매치하는 패턴. 플래그 i를 추가하면 대소문자를 구별하지 않는다.
const regExp2 = /is/i;
target.match(regExp2);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]

// 'is' 문자열과 매치하는 패턴.
// 플래그 g를 추가하면 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
const regExp3 = /is/gi;
target.match(regExp3); // -> ["Is", "is", "is"]
```

## 31.5.2 임의의 문자열 검색

- . : 임의의 문자 한 개

```js
const target = "Is this all there is?";
// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;
target.match(regExp); // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

# 31.5.3 반복 검색

{m,n} : 최소 m번, 최대 n번 반복되는 문자열 의미 (\*콤마 뒤에 공백이 있으면 동작X)
{n} : {n,n}

- : 앞선 패턴이 최소 한번 이상 반복 ---> + == {1,}
  ? : 앞선 패턴이 최대 한번(0번 포함) 이상 반복 문자열 ---> ? == {0,1}

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{1,2}/g; // 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색
target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]

const regExp2 = /A{2}/g; // 'A'가 2번 반복되는 문자열을 전역 검색
target.match(regExp2); // -> ["AA", "AA"]

const regExp3 = /A{2,}/g; // 'A'가 최소 2번 이상 반복돠는 문자열을 전역 검색
target.match(regExp3); // -> ["AA", "AAA"]

const regExp = /A+/g; // 'A'가 최소 한 번 이상 반복 문자열('A', 'AA', 'AAA', ...)을 전역 검색
target.match(regExp); // -> ["A", "AA", "A", "AAA"]

const target2 = "color colour";
// 'colo' 다음 'u'가 최대 한 번(0번포함) 이상 반복되고
// 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색
const regExp = /colou?r/g;
target.match(regExp); // -> ["color", "colour"]
```

## 31.5.4 OR 검색

| : or
[] : or
[-] : 범위

- : 분해되지 않은 단어 레벨로 검색

\d : 숫자 [0-9]
\D : \d 반대
\C : 숫자아닌 문자
\w : 알파벳, 숫자. 언더스코어 [A-Za-z0-9_]
\W : (\w와 반대)알파벳, 숫자, 언더스코어가 아닌 문자

```js
const target = "A AA B BB Aa Bb";
const regExp = /A!B/g; // 'A' 또는 'B' 전역 검색
target.match(regExp); // -> ["A", "A", "A", "B", "B", "B", "A", "B"]

// 'A' 또는 'B'가 한 번이상 반복되는 문자열을 전역 검색
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp2 = /A+|B+/g;
target.match(regExp2); // -> ["A", "AA", "B", "BB", "A", "B"]

// 위와 동일 |을 []로 변경
const regExp3 = /[AB]+/g;
target.match(regExp3); // -> ["A", "AA", "B", "BB", "A", "B"]

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색
//'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ... 'Z', 'ZZ', 'ZZZ', ...
const target2 = "A AA BB ZZ Aa Bb";
const regExp4 = /[A-Z]+/g;
target2.match(regExp4); // -> ["A", "AA", "BB", "ZZ", "A", "B"J

const target3 = "AA BB Aa Bb 12";
// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp5 = /[A-Za-z]+/g;
target3.match(regExp5); // -> ["AA", "BB", "Aa", "Bb"]

const target4 = "AA BB 12,345";
// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp6 = /[0-9]+/g;
target4.match(regExp6); // -> ["12", "345"]

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는문자열을 전역 검색
const regExp7 = /[0-9,]+/g;
target4.match(regExp7); // -> ["12,345"]

// 위 \d로 수정
let regExp8 = /[\d,]+/g;
target4.match(regExp8); // -> ["12,345"]
// '0'~'9'가 아닌 문자(숫자가 아닌문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
regExp8 = /[\D,]+/g;
target.match(regExp8); // -> ["AA BB ",","]

const target5 = "Aa Bb 12,345 _$%&";
// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색
let regExp9 = /[\w,]+/g;
target5.match(regExp9); // -> ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
regExp9 = /[\W,]+/g;
target5.match(regExp9); // -> [" ", " ", ",", " $%&"]
```

## 31.5.5 NOT 검색

[^...] : not
[^0-9] : 숫자를 제외한 문자 [0,9], \d !== \D ([0-9], \d 와 반대)
[^a-za-z0-9_] : 알파벳, 숫자, 언더스코어 아닌 문자 \W !== ([A-Za-z0-9_], \w)

```js
const target = "AA BB 12 Aa Bb";
// 숫자를 제외한문자열을 전역 검색
const regExp = /[^0-9]+/g;
target.match(regExp); // -> ["AA BB Aa Bb"]
```

## 31.5.6 시작 위치로 검색, ## 31.5.7 마지막 위치로 검색

- ^ : 문자열의 시작
- $ : 문자열의 마지막

```js
const target = "https://poiemaweb.com";
const regExp = /^https/; // 'https'로 시작하는지 검사
regExp.test(target); // -> true

const regExp2 = /com$/; // 'com'으로 끝나는지 검사
regExp.test(target); // -> true
```

# 31.6 자주 사용하는 정규표현식

## 31.6.1 특정 단어로 시작하는지 검사

'http://' 또는 'https://'로 시작하는지 검사한다.

```js
const url = "https://example.com";
// 'http://'또는'https://'로 시작하는지 검사
/^https?:\/\//.test(url); // -> true
/^(http!https):\/\//.test(url); // -> true, 위와 동일
```

## 31.6.2 특정 단어로 끝나는지 검사

```js
const fileName = "index.html";
// 'html'로 끝나는지 검사
/html$/.test(fileName); // -> true
```

## 31.6.3 숫자로만 이루어진 문자열인지 검사

처음과 끝이 숫자, 숫자가 최소 한 번 이상 반복

```js
const target = "12345";
// 숫자로만 이루어진 문자열인지 검사
/^\d+$/.test(target); // -> true
```

## 31.6.4 하나 이상의 공백으로 시작하는지 검사

하나 이상의 공백으로 시작하는지 검사
\s : 스페이스, 탭 등 [\t\r\n\v\f]

```js
const target = " Hi!";
// 하나 이상의 공백으로 시작하는지 검사
/^[\s]+/.test(target); // -> true
```

## 31.6.5 아이디로 사용 가능한지 검사

```js
const id = 'abcl23 ;
// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

## 31.6.6 메일 주소 형식에 맞는지 검사

```js
const email = "mail@domain.com";
/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.rest(
  email
); // -> true
```

인터넷 메시지 형식 규약 RFC 53228에 맞는 정교한 패턴 매칭

```js
(?:[a-z0-9!#$%&'*+/=?^_'{!}—]+(?:\.[a-z0-9!#$%&'*+/=?^_'{!}—]+)*!"(?:[\x01-\x08\x0b\x0c\x0e-\xlf\x21\x23-\x5b\x5d-\x7f]I\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?}\[(?:(?:25[0-5]!2[0-4][0-9][[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?J[a-z0-9-]*[aZ0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\xlf\x21-\x5a\x53-\x7f]![\x01-\x09\x0b\x0c\x0e-\x7f])+)\])
```

## 31.6.7 핸드폰 번호 형식에 맞는지 검사

```js
const cellphone = "010-1234-5678";
/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

## 31.6.8 특수 문자 포함 여부 검사

특수 문자 : A-Za-z0-9 이외 문자

```js
const target = "abc#123";
/[^A-Za-z0-9]/gi.test(target); // -> true

// 대체, 특수 문자를 선택적으로 검사
/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi.test(target); // -> true

// 특수 문자 제거
target.replace(/[^A-Za-z0-9]/gi, ""); // -> abc123
```
