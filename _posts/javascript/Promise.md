```javascript
//new Promise 패턴을 사용하면 아래와 같이 쓸 수 있습니다.

function async1 (param) {
    return new Promise(function(resolve, reject) {
        resolve(param*2);
    });
}
function async2 (param) {
    return new Promise(function(resolve, reject) {
        resolve(param*3);
    });
}
function async3 (param) {
    return new Promise(function(resolve, reject) {
        resolve(param*4);
    });
}
 
var start = 1;
async1(start)
    .then(async2)
    .then(async3)
    .then(result => {
        console.log(result); // 24
    });


//함수 선언 코드가 좀 길어지긴 했지만... 함수 사용 부분은 좀 더 명확해졌습니다.
//같은 내용을 Promise.resolve() 로 사용하면 아래와 같죠.
function async1 (param) {
    return Promise.resolve(param*2);
}
function async2 (param) {
    return Promise.resolve(param*3);
}
function async3 (param) {
    return Promise.resolve(param*4);
}
 
var start = 1;
async1(start)
    .then(async2)
    .then(async3)
    .then(result => {
        console.log(result); // 24
    });



```

