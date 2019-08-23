# javascript 정복 캠프 강의 



| **1주차. 자바스크립트 기본 (ES6 이전)**                      |
| ------------------------------------------------------------ |
| 자바스크립트 변수 스코프와 실행 컨텍스트자바스크립트 상속자바스크립트 고차함수**[실습] Canvas를 이용한 그래프 구현**자바스크립트 상속, 클로져, 기본 개념을 그래프 개발을 통하여 재접근 |



| **2주차. ES2015 / TypeScript 시작하기**                      |
| ------------------------------------------------------------ |
| Babel과 WebPack을 통한 개발환경 구성ES2015 시작하기TypeScript 시작하기**[실습] 베이스볼 게임 개발**ES6와 TypeScript를 활용한 베이스볼 게임 개발 |



| **3주차. 웹 컴포넌트와 컴포넌트 기반 개발 방법**             |
| ------------------------------------------------------------ |
| Web Component 시작하기 (쉐도우돔, 커스텀 엘리먼트, HTML Import, Template)Angular 컴포넌트 (React.js와 Vue.js는 별도 자료를 통한 개인 실습)**[실습] Drawer 컴포넌트 및 상품 컴포넌트 실습**Web Component와 Angular 컴포넌트 실습 |



| **4주차. NODE.JS 시작하기**                                  |
| ------------------------------------------------------------ |
| CommonJS와 Node.js에서의 모듈Express.js를 통한 REST Api 개발Node.js에서 TypeScript 사용하기MongoDB 와 Mongoose를 이용한 CRUDJSON Web Token을 이용한 인증 및 인가 처리**[실습] 자바스크립트 서버사이드 개발 실습**로그인 기능, 상품정보 조회 API 및 주문 API 구현 |



| **5주차. 자바스크립트 비동기 프로그래밍**                    |
| ------------------------------------------------------------ |
| Promise 시작하기Rxjs 시작하기**[실습] 상품관리 애플리케이션 구현**상품 카테고리별 CRUD |



| **6주차. ANGULAR로 만드는 E-COMMERCE 웹 애플리케이션 실습**  |
| ------------------------------------------------------------ |
| 단일 페이지 어플리케이션 시작하기 (라우터처리)Angular 폼 처리상품 조회 및 주문하기 실습을 통한 Rxjs 활용Angular 서비스 및 의존관계 주입상품 조회 및 주문하기 실습을 통한 Rxjs 활용(Firebase 연동은 별도 강의자료로 제공됩니다.) |



인터프린터 언어라서 엔진이 해석하고 실행 엔진은 브라우저에 설치되어있음  

nodejs는 엔진(v8) 가지고 있음  



동적타입언어  

프로토타입기반 oop  

리터럴표기 표현법 - {} () 

​    프로그래밍상에서 값을 표현하는거  

함수형프로그래밍 



원시타입 

number 

string  

boolean 

null 

undefined 



객체타입 

array 

function 

date ….. 



NaN 도 타입은 number다  

"h"*2 =NaN 



js는 소수를 java의 double 형식을 씀  

그래서 0.1+0.2 = 0.300004나옴  

(1+2)/10 = 0.3나옴 



number는 16비트로  

UTF-16인코딩이면 var e = "e"; length 2개로 나오겠지  



String 생성자함수 new키워드로 쓰지 마라 Object 로 타입 나옴 String이 아니라 -js 버그라 한다  



널체크시 주의  

보통 === 스트롱체크 하지만  

var a; //undefined 

if(a == null)  

이럴때는 == 쓴다  



**객체**

getter/setter 

_name <- private 없으니깐 부탁하는 네이밍 언더바 



JSON.stringfy(객체) <- 직렬화 

JSON.parse(json형태 문자열)<-역직렬화



객체의 값을 함수인자로 주면 call by value 

객체를 주면 call by reference 



window객체 루뜨객체 전역객체 



자바스크립트는 new 로 객체 만들면 해당 객체의 this

new안하면 this는 window를 가르킴



/ 가장 간단한 방법은 객체 리터럴을 이용 var person = { 

name: 'jay' }; 

// new 키워드를 이용하여 생성 

var obj = new Object(); // 빈 객체 생성, 리터럴 표현인 {}와 같다. var arr = new Array(); // 빈 배열 객체 생성, 리터럴 표현인 []와 같다. var date = new Date(); // 빈 현재 날짜 객체 생성 

// Object.create를 이용하여 전달 받은 객체를 상속하는 객체 생성 var obj2 = Object.create({x:1, y:2}); 



var person = { 

name: 'jay', hello: function() { 

console.log('hello') }

};

var name = person.name; // person 객체의 name 속성에 접근한다. person.age = 20; // 속성이 없으면 새로운 속성을 추가하고 값을 할당한다. person.name = 'jin'; // 있으면 기존값을 덮어쓴다. 

var age = person["age"] // person 객체는 일종의 연관배열로 배열의 요소에 접근하듯 접근할 수 있다. 

// 속성 접근 방법 #1 object.property // 속성 접근 방법 #2 object["property"] 

var numOfChildren = person.children.length; // 에러가 발생 delete person.age; // 속성 제거 person.hasOwnProperty("age"); // 속성 존재 확인 



**함수**

함수선언문 

익명함수표현식 

이름있는함수표현식 

즉각실행함수표현식 

함수생성자 



function sum(x, y) { return x + y; } 

var sum2 = function(x, y) { return x + y; }; 

var sum3 = function sum3(x, y) { return x + y; }; 

var sum4result = (function(x, y) { return x + y; })(1, 2);  

var sum5 = new Function('x', 'y', 'return x + y;'); 





var square = function(x) { // 함수를 변수에 할당한다. return x * x; 

}

var f = function factorial(x) { // 함수 표현식은 이름을 포함할 수 있고 재귀호출을 가능하게 한다. 

if (x <= 1) { return 1; 

} else { 

return x * factorial(x - 1); 

} };

data.sort(function(a, b) { return a - b; }); // 함수는 다른 함수의 인자로 전달이 가능하다. 

var tensquared = (function(x) {return x * x;}(10)); // 함수는 표현과 동시에 즉시 호출을 할 수 있다. (즉각호출패턴) 

일급객체라서 변수, 속성,인자,리턴 가능 





**스코프**

함수에만 스코프있다.  



**프레임워크/라이브러리**

리엑트는 함수형+웹컴포넌트 

앵귤러는 oop 사상 





**DOM**

노드객체들의 트리 구조를 가진다  

노드타입은 값이 숫자다  



노드인터페이스 상속구조 가짐



DOM 네비게이션  

DOM검색  

DOM Attribute는 html 태그 속성 

DOM Properties는 자바스크립트 객체의 속성   



Capturing  

Bubbling 

event.stopPropagation(); //전파막기 

event.preventDefault();//지금 이벤트 멈추기  

oop 

scope 

context  