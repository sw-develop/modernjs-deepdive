# CH9 타입 변환과 단축 평가

## 타입 변환이란?
### 1) 명시적 타입 변환 (타입 캐스팅)
개발자가 의도적으로 값의 타입을 변환하는 것   
```javascript
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str);   // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x);   // number 10 
```


### 2) 암묵적 타입 변환 (타입 강제 변환)
표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것   
```javascript
var x = 10;

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + '';  
console.log(typeof str, str);   // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x);   // number 10
```   
→ x + ''을 평가하기 위해 x 변수의 숫자 값을 바탕으로 새로운 문자열 '10'을 생성하고 이것으로 표현식 '10' + ''을 평가함      
→ 이때 암묵적으로 생성된 문자열 '10'은 x 변수에 재할당되지 않음

### 공통
→ 즉, 2가지 변환 모두 기존 원시 값을 직접 변경하는 것은 아님   
→ 원시값은 변경 불가능한 값이므로 변경할 수 없음   
→ 타입 변환 : 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것

---

## 암묵적 타입 변환
→ 자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있음   
→ 암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입을 자동 변환함   
```javascript
// 문자열 타입으로 변환 
1 + '' // "1"
`1 + 1 = ${1 + 1}` // "1 + 1 = 2"

// 숫자 타입으로 변환
1 - '1' // 0
1 * '10' // 10
1 / 'one' // NaN
+'' // 0
+'1' // 1
```

---

## 명시적 타입 변환  
명시적으로 타입을 변경하는 방법   
1.  표준 빌트인 생성자 함수(String, Number, Boolean)를 new 연산자 없이 호출하는 방법   
2.  빌트인 메서드를 사용하는 방법
3.  암묵적 타입 변환 이용하는 방법   

### 1) 문자열 타입으로 변환
```javascript
// 1. String 생성자 함수를 new 연산자 없이 호출하기
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"

// 2. Object.prototype.toString 메서드를 사용하기 (빌트인 메서드)  
(1).toString(); // "1"
(NaN).toString(); // "NaN"
(Infinity).toString(); // "Infinity"

// 3. 문자열 연결 연산자를 이용하기 (암묵적 타입 변환 방법)
1 + ''; // "1"
NaN + ''; // "NaN"
Infinity + ''; // "Infinity"
```

### 2) 숫자 타입으로 변환
```javascript
// 1. Number 생성자 함수를 new 연산자 없이 호출하기
Number('0'); // 0
Number(true); // 1

// 2. parseInt, parseFloat 함수를 사용하기(문자열만 숫자 타입으로 변환 가능) (빌트인 메서드)
parseInt('0');   // 1
parseFloat('10.11'); // 10.11

// 3. + 단항 산술 연산자 이용하기
+'0';   // 0
+true;  // 1

// 4. * 산술 연산자 이용하기   
'0' * 1;    // 0
true * 1;   // 1
```

### 3) 불리언 타입으로 변환
```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하기
Boolean('x');   // true
Boolean('');    // false

Boolean(0); // false
Boolean(1); // true

// 2. ! 부정 논리 연산자를 두 번 사용하기 
!!'x';  // true
!!'';   // false
```

---

## 단축 평가 
### 1) 논리 연산자를 사용한 단축 평가
→ 논리합(||) or 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수 있음   
→ 논리합(||) or 논리곱(&&) 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가됨   
   
→ **논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환함 == 단축 평가(short circuit evaluation)**      
→ 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말함   
→ 대부분의 프로그래밍 언어는 단축 평가를 통해 논리 연산을 수행함   

```javascript
// 논리합(||) 연산자
true || anything    // true
false || anything   // anything

// 논리곱(&&) 연산자
true && anything    // anything
false && anything   // false
```

### 활용1 - if문 대체하기
```javascript
var done = true;
var message = '';

// 1) 주어진 조건이 true일 때 - 논리곱 연산자 사용 
if (done) message = '완료';

// if 문은 단축 평가로 대체 가능함
// done이 true라면 message에 완료를 할당
message = done && '완료';
console.log(message);

// 2) 주어진 조건이 false일 때 - 논리합 연산자 사용 
done = false;

if (!done) message = '미완료';

// done이 false라면 message에 미완료를 할당
message = done || '미완료';
```

### 활용2 - 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때
→ 객체가 null or undefined인 경우 객체의 프로퍼티를 참조하면 타입 에러가 발생하고 프로그램 강제 종료됨   
```javascript
var elem = null;
var value = elem.value; // TypeError: Cannot read properties of null (reading 'value')

// 단축 평가를 통해 미리 확인하기
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가됨
var value = elem && elem.value;
```

### 활용3 - 함수 매개변수에 기본값을 설정할 때 
→ 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당됨   
→ 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있음   
```javascript
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str){
    str = str || '';    // 빈 문자열 설정
    return str.length;
}

getStringLength();  // 0
getStringLength('hi')   // 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = ''){
    return str.length;
}

getStringLength();  // 0
getStringLength('hi')   // 2
```

### 논리 연산에서 false로 평가되는 Falsy 값 
false, undefined, null, 0, -0, NaN, ''   

### 2) 옵셔널 체이닝 연산자
→ ES11(ECMAScript2020)에서 도입된 옵셔널 체이닝 연산자 ?.는 좌항의 연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어감   
→ 객체가 null 또는 undefined가 아닌지 확인할 때 유용함   

```javascript
var elem = null;
var str = '';

// 1) 옵셔널 체이닝 연산자 사용
var value = elem?.value;
console.log(value); // undefined 

// 문자열 길이를 참조함 
// 좌항의 연산자가 false로 평가되는 Falsy 값이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어감
var length = str?.length;
console.log(length);    // 0

// 2) 논리곱 연산자 사용 - 0이나 ''인 경우 false로 평가됨 
var str = '';    

// 문자열 길이 참조
var length = str && str.length; // str이 false로 평가되므로 문자열의 길이를 참조하지 못함   

// 문자열 길이 참조하지 못함 
console.log(length);    // ''
```

### 3) null 병합 연산자   
→ ES11(ECMAScript2020)에서 도입된 null 병합 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환함   
→ null 병합 연산자 ??는 변수에 기본값을 설정할 때 유용함   

```javascript
var foo = null ?? 'default String'
console.log(foo);   // default String

// Falsy 값인 0이나 ''도 기본값으로서 유효한 경우 null 병합 연산자 사용하기
var foo = '' ?? 'default String';
console.log(foo);   // ""
```



