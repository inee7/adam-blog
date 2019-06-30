# vuetify



Vuetify는 12개를 컬럼을 사용한 그리드 시스템을 사용한다. 5가지 화면 크기에 따른 유형을 적용할 수 있다.

`v-container`는 전체 너비를 기준으로 화면에 출력될 컨테이너를 중앙에 배치한다. `v-layout`은 섹션을 구분하는데 사용하고 `v-flex`를 포함한다. 대부분의 경우 `v-layout`의 `row`와 `column`을 주로 사용한다. `v-flex`의 내부에는 `div` 태그를 사용해서 필요한 내용을 추가한다.

### 기본적인 vuetify 예제

아래 예제는 vuetify에서 소개한 예제이다. 레이아웃 요소를 배치 할 땐 `v-app` 속성을 최상위에 사용해야 한다. `v-content`를 사용하면 구성 요소가 동적으로 조정된다.

```html
<v-app>
  <v-navigation-drawer app></v-navigation-drawer>
  <v-toolbar app></v-toolbar>
  <v-content>
    <v-container fluid>
      <router-view></router-view>
    </v-container>
  </v-content>
  <v-footer app></v-footer>
</v-app>
```

### Breakpoints

`xs` < `sm` < `md <`lg`<`xl` 순서로 적용된다. 화면 크기를 기준으로 적용된다.

## Typography

일반적으로 가장 많이 사용하는 폰트 관련 내용이다. Vuetify는 Roboto Font를 사용한다. `.display-4`는 `h1` 태그에 적합하고, `.title`은 `h6`에 잘 어울린다. `.body-1`은 일반적인 본문에 어울리고, 중요한 본문 내용은 `.body-2`를 사용한다. `font-weight-{thin, light, regular, medium, bold, black}`등을 사용하여 폰트를 설정할 수 있다. 색상의 경우 `{배경 글자}`형태로 구성되며, `{배경색, 배경색 밝기, 글자, 글자색 밝기}` 형태로 구성된다. 좀 더 자세한 내용은 [Application typography — Vuetify.js](https://vuetifyjs.com/en/framework/typography)를 참고하자.

```html
<p class="red white--text">Eleates de ferox quadra, promissio onus!Cur elevatus studere?Cur abactus tolerare?Cum extum studere, omnes vitaes magicae pius, castus amores.Eheu, luna!</p>
<p class="pink lighten-4 red--text text--darken-4">Audax agripetas ducunt ad poeta.Nunquam imitari fluctui.Cobaltums sunt armariums de placidus poeta.Ubi est pius olla?Lanista experimentums, tanquam lotus hibrida.</p>
<h1 class="display-1">Brevis gallus hic imperiums racana est.</h1>
<h4 class="display-4">Nunquam attrahendam bulla.Caesium de bi-color lumen, resuscitabo parma!</h4>
<p class="headline">Fuga, axona, et mortem.Superbus mensa nunquam tractares tumultumque est.Heu.</p>
<p class="subheading font-weight-bold">Sunt plasmatores transferre salvus, emeritis mensaes.</p>
<p class="caption">Sunt rectores transferre castus, germanus fermiumes.</p>
```

## Button and Icon

`flat`은 그림자와 배경이 없는 버튼을 생성하고, `depressed`는 그림자가 없으며, `fab`는 동그란 형태의 버튼을 생성한다. 대부분의 경우 아래 예제에서 손쉽게 사용할 수 있었으며, 좀 더 자세한 사항은 [Button Component — Vuetify.js](https://vuetifyjs.com/en/components/buttons)을 참고하자.

```html
<v-btn class="pink white--text">Click ME</v-btn>
<v-btn depressed color="pink">Click ME</v-btn>
<v-btn flat color="blue">Click ME</v-btn>

<v-btn depressed class="pink white--text">
  <v-icon left>email</v-icon>
  <span>E-mail me</span>
</v-btn>

<v-btn depressed small class="pink white--text">
  <v-icon left small>email</v-icon>
  <span>E-mail me</span>
</v-btn>

<v-btn depressed large class="pink white--text">
  <span>E-mail me</span>
  <v-icon right small>email</v-icon>
</v-btn>

<v-btn depressed small dark class="purple">
  <v-icon>favorite</v-icon>
</v-btn>

<v-btn fab depressed small dark class="purple">
  <v-icon>favorite</v-icon>
</v-btn>
```

## Visibility

뷰포트에 따라서 해당 요소를 출력하거나 숨길 수 있는 요소이다. `only`, `and-down`,`and-up`등으로 뷰포트 범위를 지정한다. 자세한 사항은 [Display helpers — Vuetify.js](https://vuetifyjs.com/ko/framework/display)를 참고하자.

```html
<v-btn class="hidden-md-and-down">click me1</v-btn>
<v-btn class="hidden-md-and-up">click me2</v-btn>
<v-btn class="hidden-sm-only">click me3</v-btn>
```

## Toolbar and Drawer

일반적으로 사이트를 탐색하는 가장 중요한 방법 중 하나인 `Toolbar`와 `Drawer` 사용법은 아래와 같다. [Toolbars — Vuetify.js](https://vuetifyjs.com/en/components/toolbars#toolbar), [Navigation drawer — Vuetify.js](https://vuetifyjs.com/en/components/navigation-drawers#navigation-drawer)를 참고하자.

```html
<template>
    <nav>
        <v-toolbar flat app>
            <v-toolbar-side-icon @click="drawer = !drawer" class="grey--text"></v-toolbar-side-icon>
            <v-toolbar-title class="text-uppercase grey--text">
                <span class="font-weight-light">Todo</span>
                <span>Ninja</span>
            </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn flat color="grey">
                <span>Sign Out</span>
                <v-icon right>exit_to_app</v-icon>
            </v-btn>
        </v-toolbar>

        <v-navigation-drawer app v-model="drawer" class="indigo">
            <p>test</p>
        </v-navigation-drawer>
    </nav>
</template>

<script>
    export default {
        data() {
            return {
                drawer: false
            }
        }
    }
</script>
```

## Theme

`theme`는 색상을 쉽게 변경하는 방법을 제공한다. 자세한 사항은 [Application theming — Vuetify.js](https://vuetifyjs.com/en/framework/theme#theme)를 참고하자.

```html
Vue.use(Vuetify, {
  iconfont: 'md',
  theme: {
    primary: '#9652ff',
    success: '#3cd1c2',
    info: '#ffaa2c',
    error: '#f83e70'
  }
})
```

## Lists

목차등의 정보를 표현하는데 사용한다. 리스트는 [List Component — Vuetify.js](https://vuetifyjs.com/en/components/lists#list)를 참고하자

```html
<v-list-tile v-for="link in links" :key="link.text" router :to="link.route">
    <v-list-tile-action>
        <v-icon class="white--text">{{ link.icon }}</v-icon>
    </v-list-tile-action>
    <v-list-tile-content>
        <v-list-tile-title class="white--text">{{ link.text }}</v-list-tile-title>
    </v-list-tile-content>
</v-list-tile>
```

## Spacing

`m`은 margin, `p`는 padding을 뜻하며, `x`는 `*-left, *-right`이다. 아래 예제에서 `4`는 `$spacer * 1.5` 간격을 뜻 한다. 자세한 사항은 [Spacing helpers — Vuetify.js](https://vuetifyjs.com/en/framework/spacing)을 참고하자.

```html
<v-content class="mx-4 mb-4">
    <router-view></router-view>
</v-content>
```

## Card

card 요소는 다양한 구성를 조합해서 사용할 수 있다. 기본적으로 `v-card-media`, `v-card-title`, `v-card-text`, `v-card-actions`으로 구성된다. [Cards — Vuetify.js](https://vuetifyjs.com/en/components/cards#card) 링크에 있는 예제를 잘 활용하자.

```html
<v-card flat class="text-xs-center ma-3">
    <v-responsive class="pt-4">
        image here
    </v-responsive>
    <v-card-text>
        <div class="subheading">{{ person.name }}</div>
        <div class="grey--text">{{ person.role }}</div>
    </v-card-text>
    <v-card-actions>
        <v-btn flat color="grey">
            <v-icon small left>message</v-icon>
            <span class="">Message</span>
        </v-btn>
    </v-card-actions>
</v-card>
```



# Vuetify

## 개요

본 문서에서는 [Vuetify](https://vuetifyjs.com/) 적용 및 사용 방법에 대해 알아봅니다.

## 지원하는 브라우저

- Chrome
- Firefox
- Edge
- Safari 10+
- IE11 / Safari 9 (폴리필로 지원)

## 설치

### vue-cli로 설치하기



```
npm과 yarn을 모두 지원합니다.
npm으로 설치한 Vue CLI로 프로젝트를 만들더라도 yarn을 요구하므로 yarn이 설치되어있어야 합니다.
```



**vue-cli3 설치**

```
`$ ``sudo` `npm ``install` `-g yarn``$ ``sudo` `npm ``install` `-g @vue``/cli`
```



error eacces permission denied access '/usr/local/lib/node_modules'



Mac 터미널에서 **npm install -g** 전역 설치시 발생하는 오류입니다. **sudo**로 설치하면 정상 실행됩니다.



**vue-cli3 설치 확인**

```
`$ npm list -g --depth=0``/usr/local/Cellar/node/10``.11.0``/lib``├── @vue``/cli``@3.0.4``├── npm@6.4.1``└── yarn@1.10.1`
```



**Vue 프로젝트 생성**

```
`$ vue create my-app``$ ``cd` `my-app`
```



**Vue 프로젝트에 Vuetify 적용**

```
`$ vue add vuetify`
```



**Vue 프로젝트 실행**

```
`$ npm run serve`
```

### 기존 프로젝트에 적용하기



vuetify를 기존 프로젝트에 적용하려면 vuetify를 node_modules에 반드시 설치해야 합니다. 
npm 또는 yarn을 사용할 수 있으며 본 문서에서는 **npm**을 사용합니다.



**Vue 프로젝트 생성**

```
`$ vue create my-app-npm`
```



**Vuetify 설치**

```
`$ npm ``install` `vuetify --save`
```



**Vuetify 적용**

```
`//` `main.js``import` `Vue from ``'vue'``import` `Vuetify from ``'vuetify'`         `//` `Vuetify``import` `'vuetify/dist/vuetify.min.css'` `//` `Vuetify CSS 파일` `Vue.use(Vuetify)`
```



**Material Icon 설치**

```
`$ npm ``install` `material-design-icons-iconfont`
```



**Material Icon 적용**

```
`//` `main.js``import` `'material-design-icons-iconfont/dist/material-design-icons.css'` `//` `Ensure you are using css-loader`
```



아이콘 정보



https://material.io/tools/icons/

##  어플리케이션 레이아웃

### 기본 어플리케이션 마크업

- 어플리케이션이 정상적으로 작동하기 위해서는 반드시 어플리케이션을 **v-app**으로 감싸야합니다.
- **v-app** 컴포넌트는 레이아웃의 그리드 **브레이크포인트를 결정**하기 위해 필요합니다.
- 바디(body)안의 어디에 있어도 되지만 반드시 다른 모든 **Vuetify 컴포넌트들의 부모**여야합니다.


다음은 Vuetify를 사용하는 기본 어플리케이션 마크업 예제입니다.

- app 속성을 적용하면 레이아웃의 요소들을 어디든지 둘 수 있습니다.
- 가장 핵심적인 컴포넌트는 **v-content**입니다. 이 컴포넌트는 **app** 컴포넌트들의 구조에 따라 **동적으로 크기가 정해집니다.** (이를 통해 커스터마이즈된 솔루션을 만들 수 있습니다.)

```
`<``template``>``  ``<``div` `id``=``"app"``>``    ``<``v-app``>``      ``<``v-navigation-drawer` `app></``v-navigation-drawer``>``      ``<``v-toolbar` `app></``v-toolbar``>``      ``<``v-content``>``        ``<``v-container` `fluid>``          ``<``router-view``></``router-view``>``        ``</``v-container``>``      ``</``v-content``>``      ``<``v-footer` `app></``v-footer``>``    ``</``v-app``>``</``template``>`
```

- **app** prop을 적용하면 자동으로 position: fixed가 레이아웃 요소에 적용됩니다.  
- 만약, absolute 요소가 필요하다면 **absolute** porp으로 바꿀 수 있습니다.

## app의 모든 것

- Vuetify에서 **v-app** 컴포넌트와 app은 어플리케이션과 **v-content**가 적절한 크기를 가질 수 있도록 도와줍니다.
- 이를 활용하여 레이아웃을 관리하는 고통 없이 유니크한 인터페이스를 만들 수 있습니다.
- **v-app** 컴포넌트는 모든 어플리케이션에 **반드시 필요**하며, 여러 Vuetify 컴포넌트들과 기능의 시작점(마운트 포인트) 입니다.

## 그리드 시스템([Grid System](https://vuetifyjs.com/ko/layout/grid))

### 개요

- Vuetify 12 포인트 그리드 시스템을 사용합니다.
- **flex-box**는 어플리케이션 컨텐츠의 레이아웃에 사용됩니다.
- 다양한 화면 크기와 방향에 대응하는 5가지 타입의 미디어 브레이크포인트가 있습니다.
- 그리드 컴포넌트의 prop들은 정의된 속성에 따라 파생되는 (css) 클래스들입니다.

### 사용법

- **v-container**: center focused page(중앙 집중형 페이지)에 사용

- **fluid**: prop로 전체 너비로 확장 가능

- **v-layout**: 섹션을 분리하기 위해 사용

-   레이아웃 구조:

   

  v-container

   \>>

   

  v-layout

   \>>

   

  v-flex

  - 그리드 체인의 각 부분은 **flex-box** 요소
  - 마지막 **v-flex**는 자동으로 자식 요소들을 **flex: 1 1 auto**로 설정

## 브레이크포인트([Breakpoints](https://vuetifyjs.com/ko/layout/breakpoints))

- Vuetify는 12 포인트 그리드 시스템을 갖고있습니다.

- **flex-box**를 사용하여 만들고 그리드는 앱의 내용으로 사용됩니다.

- 특정 화면 사이즈와 방향을 위해 5가지 타입의 미디어 브레이크포인트가 있습니다.

  | Device      | Code | Types              | Range         |
  | :---------- | :--- | :----------------- | :------------ |
  | Extra Small | xs   | 소~대형 스마트폰   | ~600px        |
  | Small       | sm   | 소~중형 테블릿     | 600px~960px   |
  | Medium      | md   | 대형 테블릿~노트북 | 960px~1264px  |
  | Large       | lg   | 데스크탑           | 1264px~1904px |
  | Extra Large | xl   | 4k & 울트라 와이드 | 1904px~       |

### 브레이크 포인트 객체

Vuetify는 **$vuetify.breakpoint** 객체를 이용하여 어플리케이션의 브레이크포인트를 전환할 수 있습니다.

```
`export ``default` `{``  ``mounted () {``    ``console.log(``this``.$vuetify.breakpoint)``  ``},``  ``computed: {``    ``imageHeight () {``      ``switch` `(``this``.$vuetify.breakpoint.name) {``        ``case` `'xs'``: ``return` `'220px'``        ``case` `'sm'``: ``return` `'400px'``        ``case` `'md'``: ``return` `'500px'``        ``case` `'lg'``: ``return` `'600px'``        ``case` `'xl'``: ``return` `'800px'``      ``}``    ``}``  ``}``}`
```

## 디스플레이([Display](https://vuetifyjs.com/ko/layout/display))

### Visibility

- 현재 뷰포트에 따라 조건부로 보여줍니다.
- 브레이크포인트는 뷰포트의 크기입니다.
- 클래스 형식: **hidden-{breakpoint}-{condition}**
- breakpoint
  - **xs**: 소~대형 스마트폰
  - **sm**: 소~중형 테블릿
  - **md**: 대형 테블릿~노트북
  - **lg**: 데스크탑
  - **xl**: 4k & 울트라 와이드
- condition
  - **only**: xs~xl까지 해당 뷰포트 크기에서만 숨깁니다.
  - **and-down**: sm~lg까지 해당 브레이크포인트보다 **작거나 같은 크기**에서 요소를 숨깁니다.
  - **and-up**: sm~lg까지 해당 브레이크포인트보다 **크거나 같은 크기**에서 요소를 숨깁니다.

### Overflow

- 요소의 overflow 속성을 지정합니다.
- 클래스 형식: **overflow-{axis}-hidden / overflow-hidden**
- axis
  - y: y축에서 overflow를 숨깁니다.
  - x: x축에서 overflow를 숨깁니다.
- overflow-hidden
  - x축과 y축에서 overflow를 숨깁니다.

### Display

- 요소의 disply 속성을 지정합니다.

- 클래스 형식:

   

  d-{disply}

  - display
    - **inline-flex**: 요소의 display 속성을 inline-flex로 지정
    - **flex**: 요소의 display 속성을 flex로 지정
    - **inline-block**: 요소의 display 속성을 inline-block으로 지정
    - **block**: 요소의 display 속성을 block으로 지정
    - **inline**: 요소의 display 속성을 inline으로 지정

## 색상([Colors](https://vuetifyjs.com/ko/style/colors))

stylus 와 javascript를 통해 [Material Design Spec](https://material.io/guidelines/style/color.html)에 있는 모든 색상을 바로 사용할 수 있습니다. 
이 값들은 스타일 시트, 컴포넌트 파일 그리고 실제 컴포넌트 안에서 color class 시스템을 통해 사용될 수 있습니다.

### 클래스



머테리얼 색상표는 [여기](https://vuetifyjs.com/ko/style/colors#material-colors)에서 확인하세요.

- color
  - background
    - **<div class="red">**
  - text
    - **<span class="blue--text">**
- lighten / darken
  - background
    - **<div class="red lighten-1">**
  - text
    - **<span class="blue--text text--lighten-1">**

### 자바스크립트 색상 팩

Vuetify는 추가로 어플리케이션 내에서 임포트해서 사용할 수 잇는 자바스크립트 색상 팩을 지원합니다. 
이는 어플리케이션 테마를 정의하는데 사용될 수 있습니다.

```
`// main.js``import Vue from ``'vue'``import Vuetify from ``'vuetify'` `// Helpers``import colors from ``'vuetify/es5/util/colors'` `Vue.use(Vuetify, {``  ``theme: {``    ``primary: colors.red.darken1, ``// #E53935``    ``secondary: colors.red.lighten4, ``// #FFCDD2``    ``accent: colors.indigo.base ``// #3F51B5``  ``}``})`
```

## 테마([Theme](https://vuetifyjs.com/ko/style/theme))

- 프로그래밍 방식으로 어플리케이션의 색상을 쉽게 변경할 수 있습니다.
- 기본 스타일 시트를 다시 빌드하고 필요에 따라 프레임워크의 다양한 설정을 커스터마이즈 하세요.
- 테마 생성기를 원하시면 [여기](https://vuetifyjs.com/theme-generator)를 방문하세요.

## Light & Dark

- Vuetify는 머테리얼 디자인 스펙의 light와 dark 변화를 모두 지원합니다.
- 이 설정은 root 어플리케이션 컴포넌트 **v-app** 부터 대부분의 주요 컴포넌트들을 지원합니다. 
- 기본 설정은 **light** 테마이며 **dark** prop을 추가해서 바꿀 수 있습니다.

## Customizing

기본적으로 표준 테마가 모든 컴포넌트에 적용됩니다.

```
`{``  ``primary: ``'#1976D2'``,``  ``secondary: ``'#424242'``,``  ``accent: ``'#82B1FF'``,``  ``error: ``'#FF5252'``,``  ``info: ``'#2196F3'``,``  ``success: ``'#4CAF50'``,``  ``warning: ``'#FFC107'``}`
```

이는 theme 속성으로 Vue.use 함수를 사용하면 쉽게 변경할 수 있습니다. 
또한, 기본 테마의 속성을 상속하면서 동시에 모든 혹은 일부 테마 속성을 골라서 변경할 수 있습니다.

**RGB 값 사용**

```
`Vue.use(Vuetify, {``  ``theme: {``    ``primary: ``'#3f51b5'``,``    ``secondary: ``'#b0bec5'``,``    ``accent: ``'#8c9eff'``,``    ``error: ``'#b71c1c'``  ``}``})`
```

**미리 정의된 머테리얼 색상표 사용**

```
`import colors from ``'vuetify/es5/util/colors'` `Vue.use(Vuetify, {``  ``theme: {``    ``primary: colors.purple,``    ``secondary: colors.grey.darken1,``    ``accent: colors.shades.black,``    ``error: colors.red.accent3``  ``}``})`
```

내부적으로 Vuetify는 이 값들을 기반으로 DOM에서 사용될 수 있는 css 클래스들을 생성합니다. 

이 클래스들은 **primary** 또는 **secondary--text** 같은 다른 헬퍼 클래스들과 같은 형식으로 쓰입니다.


또한, 이 값들은 **$vuetify** 인스턴스의 theme 속성아래 오브젝트로 설정할 수 있습니다. 이는 테마를 동적으로 바꾸는 것을 가능하게합니다. 
내부적으로는 Vuetify는 테마 클래스들을 재생성하고 어플리케이션 중단 없이 업데이트 되도록 클래스를 업데이트 합니다.