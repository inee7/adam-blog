```javascript
//기존의 function sample
var double = function(x){
    return x*2;
}
//arrow function sample
const double = (x) => {
    return x*2;
}
//매개변수가 1개인 경우 소괄호 생략 가능
const double = x => {return x*2}
//함수가 한줄로 표현가능하면 중괄호 생략 가능하고 자동으로 return됨
const double = x => x * 2
//매개변수가 두 개 이상일 때
const add = (x,y) => x + y
//매개변수가 없을 때
() => {    ... }
//객체변환할 때 중괄호 사용
() => {return {a:1};}
//return 바로할땐 소괄호 사용
() => ({a:1})
```

