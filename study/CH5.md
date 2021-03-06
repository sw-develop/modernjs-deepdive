## CH5 표현식과 문

### 5.1 값
값 : 식이 평가되어 생성된 결과를 말함

~~~javascript
// 10 + 20은 평가되어 숫자 값 30을 생성한다.
10 + 20; // 30

// 변수에는 10 + 20이 평가되어 생성된 숫자 값 30이 할당된다.
var sum = 10 + 20;
~~~

### 5.2 리터럴
리터럴 : 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법을 말함
~~~javascript
// 숫자 리터럴 3
3
~~~
→ 사람이 이해할 수 있는 아라비아 숫자를 사용해 숫자 리터럴 3을 코드에 기술하면 자바스크립트 엔진은 이를 평가해 숫자 값 3을 생성함   
→ 자바스크립트 엔진은 코드가 실행되는 시점인 **런타임에 리터럴을 평가해 값을 생성함** 

### 5.3 표현식
표현식 : 값으로 평가될 수 있는 문, 표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조함

~~~javascript
// 리터럴 표현식
10
'Hello'

// 식별자 표현식 (선언이 이미 존재한다고 가정)
sum
person.name
arr[1]

// 연산자 표현식
10 + 20
sum = 10
sum !== 10

// 함수&메서드 호출 표현식 (선언이 이미 존재한다고 가정)
square()
person.getName()
~~~
→ 표현식은 값으로 평가됨   
→ 문법적으로 값이 위치할 수 있는 자리에는 표현식도 위치할 수 있음 

### 5.4 문
문 : 프로그램을 구성하는 기본 단위이자 최소 실행 단위    
→ 문의 집합 = 프로그램, 문을 작성하고 순서에 맞게 나열하는 것 = 프로그래밍    
→ 명령문이라고도 부름   
→ 선언문, 할당문, 조건문, 반복문 등으로 구분 가능함

토큰 : 문법적인 의미를 가지며, 문법적으로 더 이상 나눌 수 없는 코드의 기본 요소를 의미함   
→ 문은 여러 토큰으로 구성됨

~~~javascript
// 변수 선언문 
var x;

// 할당문
x = 5;

// 함수 선언문
function hello () {}

// 조건문
if (x > 1) { console.log(x); }

// 반복문 
for (var i = 0; i < 2; i++) { console.log(i); }
~~~

### 5.5 세미콜론과 세미콜론 자동 삽입 기능
→ 자바스크립트 엔진은 세미콜론으로 문이 종료한 위치를 파악하고, 순차적으로 하나씩 문을 실행함   
→ 단, if문, for문, 함수 등의 코드 블록은 언제나 문의 종료를 의미하는 자체 종결성(self closing)을 가지므로 세미콜론을 붙이지 않음   

→ 이때 세미콜론은 생략이 가능함   
→ 자바스크립트 엔진이 소스코드를 해석할 때 문의 끝이라고 예측되는 지점에 세미콜론을 자동으로 붙여주는 세미콜론 자동 삽입 기능(ASI : Automatic Semicolon Insertion)이 암묵적으로 수행됨   

→ But 세미콜론 자동 삽입 기능의 동작과 개발자의 예측이 일치하지 않는 경우가 있으므로 주의해야 함!
~~~javascript
function hello () {
    return
    {}
    // ASI의 동작 결과 => return; {};
    // 개발자의 예측 => return {};
}
~~~

### 5.6 표현식인 문과 표현식이 아닌 문
문에는 표현식인 문과 표현식이 아닌 문이 있음
→ 표현식인 문 : 값으로 평가될 수 있는 문   
→ 표현식이 아닌 문 : 값으로 평가될 수 없는 문   

위의 2가지 문을 구별하는 방법 : 변수에 할당해보기   
→ 표현식인 문은 값으로 평가되므로 변수에 할당이 가능함   
→ 표현식이 아닌 문은 값으로 평가할 수 없으므로 변수에 할당하면 에러가 발생함   
~~~javascript
// 표현식이 아닌 문은 값처럼 사용 X
var x; // 변수 선언문은 표현식이 아닌 문 
var foo = var x; // Syntax Error: Unexpected token var  

x = 100; // 할당문은 표현식인 문
var foo = x = 100; // 값처럼 사용 가능 
// 
~~~

