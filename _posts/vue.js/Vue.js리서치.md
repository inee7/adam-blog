# VueJS

 학습 비용이 작고 안정적이고 데이타 바인딩이 쉽고 화면에만 집중할 수 있음 

Vue.js는 발음대로 철저히 뷰(View)에 최적화된 프레임워크이다. 컨트롤러 대신 뷰 모델을 가지는 **MVVM(Model-View-ViewModel) 패턴**을 기반으로 디자인되었으며, **컴포넌트(Components)**를 사용하여 재사용이 가능한 UI들을 묶고 뷰 레이어를 정리하는 것을 가장 강력한 기능으로 꼽는다. 또한 **템플릿(Template)** 위주의 개발을 권장

## 기존 불편한점

JSTL에서 객체 재사용하거나 조작 어려움

 

### 파라미터로 가지고 있어야 할 변수들은 hidden등으로 데이터 summit을 위한 변수를 가지고 있어야 한다. 

```html
<input type="hidden" id="pageno" name="pageno" value="${pageno}" />
<input type="hidden" id="product" value="${product}" />
<input type="hidden" id="price" value="${price}" />

```

```javascript
$("#pageno").val($("#pageno").val());
$("#price").val($("#price").val());
$("#product").val($("#product").val()); 

```



### Api 데이터와 화면에서 보여주는 형태의 데이터가 달라서, 데이터의 조작이 필요할때 

```jsp
 <c:choose>
    <c:when test="${not empty result.cash}">
        <fmt:formatNumber value="${fn:replace(result.price , ',', '')- fn:replace(result.cash , ',', '')}" type="number" var="cash"/>${cash }만원
    </c:when>
    <c:otherwise>-</c:otherwise>
</c:choose>

```



### 비동기 호출시 화면이 바뀌는 것을 추적하기 힘듬 

```javascript
var tmp ="";
var $caseItem = $("#case_recentItem").clone();
$caseItem.find("[data-id='recent_data']").text(data1+" "+data2+" ("+data3+" 만원)");
tmp += $caseItem.html();	
```



### 화면을 위한 jstl 코드가 섞여 있음 

```jsp
 <c:choose>
       <c:when test="${useYN ne 'Y' and not empty result }">
       <div class="section"><div>
       </c:when>
       <c:otherwise>
       </c:otherwise>
   </c:choose>

```



### 화면 구조 바뀔 때 모든 selector을 확인해야함 

작은 코드 고치기 위해 전체 코드 이해해야함



```html
<div class="info">
    <span>상품가격</span>
    <span class="price">1000</span>
</div>

=> 가격은?
$("div.info > span.price").val(20000);

```







## Vue.js

```vue
 <div class="info">
     <span>상품가격</span>
     <span class="price"></span>
 </div>

 // 동일하게 사용 가능
  var info = new Vue({
    data : {
      price : 1000
    },
    methods : {
      changePrice : function(){
         this.price = 20000;
    }
 });

```







## 장점

1. 학습 비용이 적음 (가장 큰 장점!!!)

   - 자바스크립트에 대한 기본 지식만 있으면, 3시간 안에 학습하고 실무에서 사용 가능 (딱히 교육이 필요가 없음)

   - 책을 따로 살 필요가 없음

2. 요구사항 변화에 따라서(특히 디자인) 빠르게 적용 가능

   - 화면 디자인이 자주 바뀌는 프로젝트의 경우 매우 유용

   - 바인딩할 데이터만 바꿔주면 되는 구조이므로, 유지보수 시간이 줄어듦

3. 읽기 쉬운 구조화된 코드 및 자바스크립트 객체 재사용이 매우 유용

   - 특히, selector, 데이터 바인딩(data), 기능(methods)이 나누어져있어서 편하게 바인딩하고 활용할 수 있다.

   - 개발자마다 코딩 스타일이 다를 수 있지만, 대부분 소스가 비슷한 구조를 가지게 되어서 다른 사람 소스를 유지보수 시 단시간 내에 유지보수를 할 수 있음

   - 자바스크립트 객체를 재사용하기가 매우 유용하므로, 코드의 양이 줄어듦. 특히, 변경된 데이터의 추적을 매번 할 필요가 없다는 것이 매우 좋음.

   - vue.js 예

     ```javascript
     var patient =  new Vue({
            el: '.container',    //selector : jQuery selector 동일
            data : {    //바인딩할 JSON 데이터
              name : 'gil-dong',
              surname : 'hong',
              price : '20000',
              gender : [{
                kCode : '남',
                eCode : 'M'
              },
              {
                kCode : '여',
                eCode : 'M'
              }],
              genderSelected : '',
              phoneNumber : ''
            },
            computed : {
              fullName :  function () {
                //성 + 이름으로 이름을 표시한다.
                return this.name + " " + tihs.surname; //    this는 patient 변수 객체(data)
              }
            },
            methods : {
               registerPatient : function () {
                 //전화번호
                 //환자를 등록한다.
               },
              //전화번호를 업데이트한다.
               updatePhoneNumber :  function(){
                 var param = {"phoneNumber", "010-1234-5678"};
                 param = JSON.parse(param);
                 param.name = 'test2';
                 param = JSON.stringify(param);
                 //서버로 데이터 전송시, hidden tag, form 태그의 사용이 필요가 없다.
              $.ajax({
               url : '/register/patient',
               type: "POST",
               data : param,
               dataType : "json",
               contentType: "application/json",
               success: function (data) {
                 alert("성공!")
               });
               }
               setTenThoushand : function(){
                //3자리 단위로 콤마표시를 한다.
               }
            }
     
     ```

     

4. 한글 레퍼런스 잘 되어 있음

   - <https://kr.vuejs.org/v2/guide/index.html>





참고

- 확장 가능한 모듈 사용 -> 참고 : <https://github.com/vuejs/awesome-vue>



1. 비동기 라이브러리 axios는 IE 고려안함. 호환성 고려하기 위해서 jQuery에서 제공하는 ajax 모듈 사용하는 것이 더 나음.
2. 자바스크립 객체로 조작할 수 있기 때문에 lodash 라이브러리를 사용하는 것이 좋다.
3. 최소 스펙 IE7까지 고려해야하는 프로젝트에서는 적합하지 않다.
   - 모바일의 경우 안드로이드 갤럭시3 까지 지원한다.
4. 가끔 jQuery가 필요한 상황이 있긴하다. 상황에 맞추어서 jQuery도 적절하게 사용해서 사용하면 좋다.
5. Vue.js 가 객체가 로딩되는 시간과 각 스크립트가 실행되는 우선순위에 대해서 이해할 필요가 있다.
6. 크롬에 있는 디버깅 툴도 나름 유용하다.
   - <https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?utm_source=chrome-ntp-icon>





ref) [vuejs경험후기공유](http://tech.javacafe.io/2017/11/14/Vuejs-경험-후기-공유/)

[vue.js 토스트 밋업](https://meetup.toast.com/posts/99)

[vue.js 입문서](https://joshua1988.github.io/web-development/vuejs/vuejs-tutorial-for-beginner/)



## 환경구축

[webpack템플릿으로 vue.js환경 구축](https://itstory.tk/entry/vuecli-Webpack-%ED%85%9C%ED%94%8C%EB%A6%BF%EC%9C%BC%EB%A1%9C-vuejs-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)

[스프링부트와 연동](https://itstory.tk/entry/Spring-Boot-Vuejs-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0)

[Vue.js Frontend with a Spring Boot Backend feat.thymeleaf](https://www.baeldung.com/spring-boot-vue-js)



## vue.js 설치 방법 

CDN으로 페이당 설치 가능 

대규모는 vue-cli로 선택 

vue cli 템플릿 선택 필요

[설치방법](https://beomy.tistory.com/39)



### 그렇다면 CDN스크립트 방식과 CLI 방식의 차이는??

> CDN으로 vue를 가져와도 vue의 기능을 모두 사용할 수 있지만 
> CDN도 결국 서버에 보내는 하나의 request 입니다. 브라우저는 한 시점에 5개의 request를 요청할 수 있습니다. CDN으로 가져오면 request가 하나 늘어나는 것이나 다름 없으므로 url로부터 리소스를 가져올 것이 많은 경우에는 결과적으로 페이지를 볼 수 있을 때까지 조금 더 기다리게 되겠죠.
>
> 대규모 응용 프로그램 같은 경우에는 js모듈이 많기 때문에 대부분 module bundler 를 이용하게 되죠. NPM으로 vue를 땡겨서 webpack 같은 걸로 관리하게 되는데, Vue CLI로 가져오게 되면 이런것 크게 신경쓸 것 없이 미리 다 세팅이 되어 있습니다. 그리고 개발시에 핫 리로드, lint-on-save 이런 기능 대부분 쓰실 텐데 이런 설정도 싹 다 되어있죠.
>
> webpack 같은 모듈 번들러만 쓰는 경우에는 특별히 세팅해주시지 않으면 빌드 후에 app.js 와 같이 모든 js가 하나의 파일로 묶이는데 이게 용량이 금방 늘어납니다. chunk로 split 해주는걸 모르면 js파일만 10MB 넘어가는 거 순식간이더라구요.
>
> 저희 회사는 nuxt 사용하는데 code splitting 까지 자동으로 되는 것 같습니다. 설정하는 데 시간 거의 안쓰고 곧바로 개발할 수 있는 점이 참 좋은 것 같아요.



> **script 태그로 참조하여 사용하는 방법으로는 SPA(Single Page Application) 앱을 개발할 수 없기 때문에 대부분 Vue CLI 도구를 이용합니다.**
>
> Vue CLI를 이용해 개발하면 .vue 파일로 분리된 단일 파일 컴포넌트 단위로 컴포넌드 중심의 개발을 합니다. 개발된 컴포넌트를 조합하여 UI를 구성하지요. 개발된 코드는 webpack에 의해서 빌드되고 번들링된 후 배포버전의 html, css, js 파일을 떨궈 냅니다. 단 하나의 html 파일로 만들어내는거지요. 스크립트 태그로 참조하는 방식은 이게 불가능해요.



# 외부라이브러리

[부트스트랩추가](https://lovemewithoutall.github.io/it/install-bootstrap-for-vue)

[vue트리구조](https://github.com/shuiRong/vue-drag-tree)

[vuetify사용기](<https://lovemewithoutall.github.io/it/vuetify/>)

[vue awesome](<https://github.com/vuejs/awesome-vue>)

[vue cli 3.0 사용법](<http://www.daleseo.com/vue-cli3>)

[vuex 사용법](https://joshua1988.github.io/web-development/vuejs/vuex-start)

[머티리얼아이콘](<https://material.io/tools/icons/?style=baseline>)

[vue router+vuetify](<https://embed.plnkr.co/plunk/HFL4Pk6ohOM7lwRC>)