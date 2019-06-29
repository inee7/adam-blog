`v-bind` 속성은 **디렉티브** 이라고 합니다. 디렉티브는 Vue에서 제공하는 특수 속성임을 나타내는 `v-` 접두어가 붙어있으며 사용자가 짐작할 수 있듯 렌더링 된 DOM에 특수한 반응형 동작을 합니다

사용자가 앱과 상호 작용할 수 있게 하기 위해 우리는`v-on` 디렉티브를 사용하여 Vue 인스턴스에 메소드를 호출하는 이벤트 리스너를 첨부 할 수 있습니다 

`v-if` `v-for`

Vue는 또한 양식에 대한 입력과 앱 상태를 <u>양방향</u>으로 바인딩하는 `v-model` 디렉티브를 제공합니다.



싱글파일컴포넌트 

```vue
Test.vue

<template>
	<span v-bind:title="message">
    내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
  </span>
	<p v-if="seen">
    이제 나를 볼 수 있어요
  </p>	
	<li v-for="todo in todos">
      {{ todo.text }}
  </li>
	<button v-on:click="reverseMessage">메시지 뒤집기</button>
 
	<p>{{ message }}</p>
  <input v-model="message">
</template>

<script>
  export default {
    data: () => ({
      message: "마우스 올리면 나오는 메시지",
      seen: true,
      todos: [
       { text: "1" },
       { text: "2" }
      ]
    }),
    props: ['todo']
  }
</script>

```



```vue
<template>
<test 
      v-for="item in groceryList"
      :todo="item"
      :key="item.key"> 
</test>
</template>
<script>
  export default {
    data: () => ({
      groceryList: [
        { id: 0, text: 'Vegetables' },
        { id: 1, text: 'Cheese' },
        { id: 2, text: 'Whatever else humans are supposed to eat' }
      ]
  }
</script>


```





### [`v-bind` 약어](https://kr.vuejs.org/v2/guide/syntax.html#v-bind-%EC%95%BD%EC%96%B4)

```vue
<!-- 전체 문법 -->
<a v-bind:href="url"> ... </a>

<!-- 약어 -->
<a :href="url"> ... </a>
```

### [`v-on` 약어](https://kr.vuejs.org/v2/guide/syntax.html#v-on-%EC%95%BD%EC%96%B4)

```vue
<!-- 전체 문법 -->
<a v-on:click="doSomething"> ... </a>

<!-- 약어 -->
<a @click="doSomething"> ... </a>
```

슬롯(slot)은 컴포넌트의 재사용성을 높여주는 기능입니다. 특정 컴포넌트에 등록된 하위 컴포넌트의 마크업을 확장하거나 재정의할 수 있습니다

```html
<!-- ButtonTab.vue -->
<template>
  <div class="tab panel">
    <!-- 탭 헤더 -->
    <slot></slot>
    <!-- 탭 본문 -->
    <div class="content">
      Tab Contents
    </div>
  </div>
</template>


HTML
Copy
```

위 코드는 ButtonTab 컴포넌트의 코드입니다. 탭을 구현한다고 생각하고 탭 헤더와 본문을 구분하는 태그를 작성하였습니다. 여기서 탭 헤더에 들어갈 구체적인 태그를 정하지 않고 일단 `<slot>` 태그로 빈 칸을 남겨놉니다. 만약 이 컴포넌트를 등록한 상위 컴포넌트에서 `<slot>` 태그 영역을 구현하지 않으면 해당 부분은 공백으로 표시됩니다.

`<slot>` 태그의 위치에 주목하면서 ButtonTab 컴포넌트를 TabContainer 컴포넌트의 하위 컴포넌트로 등록합니다.

```html
<!-- TabContainer.vue -->
<template>
  <button-tab>
    <!-- slot 영역 -->
    <h1>First Header</h1>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1>Second Header</h1>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1>Third Header</h1>
  </button-tab>
</template>

<script>
export default {
  components: {
    ButtonTab
  }
}
</script>


HTML
Copy
```

TabContainer 컴포넌트에 ButtonTab 컴포넌트를 등록하고 ButtonTab 컴포넌트를 세 곳에 표시했습니다. 여기서 `<button-tab>` 컴포넌트 태그의 안에 각기 다른 헤더의 내용을 정의했습니다. 만약 ButtonTab 컴포넌트에 `<slot>` 태그를 정의하지 않았다면 컴포넌트를 등록하는 시점에 마크업을 재정의할 수는 없었을 것입니다.

이처럼 슬롯을 사용하면 컴포넌트의 특정 마크업 영역을 재정의하여 같은 컴포넌트를 각기 다르게 표현할 수 있습니다.


목차[들어가며](https://joshua1988.github.io/web-development/vuejs/slots/#%EB%93%A4%EC%96%B4%EA%B0%80%EB%A9%B0)[Slot](https://joshua1988.github.io/web-development/vuejs/slots/#slot)[Named Slot](https://joshua1988.github.io/web-development/vuejs/slots/#named-slot)[마무리](https://joshua1988.github.io/web-development/vuejs/slots/#%EB%A7%88%EB%AC%B4%EB%A6%AC)[글보다 더 쉽게 배우는 온라인 강좌](https://joshua1988.github.io/web-development/vuejs/slots/#%EA%B8%80%EB%B3%B4%EB%8B%A4-%EB%8D%94-%EC%89%BD%EA%B2%8C-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EC%98%A8%EB%9D%BC%EC%9D%B8-%EA%B0%95%EC%A2%8C)

## 들어가며

안녕하세요. 오랜만에 블로그 글을 씁니다. 요즘 통 바빠서 블로그 관리를 못했네요. 안 그래도 얼마 전에 State of JS 2018을 보면서 아직도 전 세계의 많은 개발자들이 Vue.js에 많은 관심을 갖고 있다는 것을 깨달았습니다. 지금도 주변에 꽤 많은 분들이 Vue.js로 새롭게 웹 서비스를 구축하고 계시는 것 같아요.

그래서 오늘은 실제로 서비스를 구현하고 계신 분들이 재미있어 할 만한 Vue.js 글을 적어보려고 합니다. 컴포넌트를 재 사용하는 방법에 대해서 시리즈로 연재해보려고 해요. 첫 번째 시리즈는 컴포넌트의 마크업을 확장하는 방법인 slot입니다.

그럼 재밌게 보시고 재밌게 코딩하시는데 도움 되었으면 좋겠습니다 :) 
Happy Coding!

## Slot

슬롯(slot)은 컴포넌트의 재사용성을 높여주는 기능입니다. 특정 컴포넌트에 등록된 하위 컴포넌트의 마크업을 확장하거나 재정의할 수 있습니다. 바로 코드로 살펴보겠습니다.

```html
<!-- ButtonTab.vue -->
<template>
  <div class="tab panel">
    <!-- 탭 헤더 -->
    <slot></slot>
    <!-- 탭 본문 -->
    <div class="content">
      Tab Contents
    </div>
  </div>
</template>


HTML
Copy
```

위 코드는 ButtonTab 컴포넌트의 코드입니다. 탭을 구현한다고 생각하고 탭 헤더와 본문을 구분하는 태그를 작성하였습니다. 여기서 탭 헤더에 들어갈 구체적인 태그를 정하지 않고 일단 `<slot>` 태그로 빈 칸을 남겨놉니다. 만약 이 컴포넌트를 등록한 상위 컴포넌트에서 `<slot>` 태그 영역을 구현하지 않으면 해당 부분은 공백으로 표시됩니다.

`<slot>` 태그의 위치에 주목하면서 ButtonTab 컴포넌트를 TabContainer 컴포넌트의 하위 컴포넌트로 등록합니다.

```html
<!-- TabContainer.vue -->
<template>
  <button-tab>
    <!-- slot 영역 -->
    <h1>First Header</h1>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1>Second Header</h1>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1>Third Header</h1>
  </button-tab>
</template>

<script>
export default {
  components: {
    ButtonTab
  }
}
</script>


HTML
Copy
```

TabContainer 컴포넌트에 ButtonTab 컴포넌트를 등록하고 ButtonTab 컴포넌트를 세 곳에 표시했습니다. 여기서 `<button-tab>` 컴포넌트 태그의 안에 각기 다른 헤더의 내용을 정의했습니다. 만약 ButtonTab 컴포넌트에 `<slot>` 태그를 정의하지 않았다면 컴포넌트를 등록하는 시점에 마크업을 재정의할 수는 없었을 것입니다.

이처럼 슬롯을 사용하면 컴포넌트의 특정 마크업 영역을 재정의하여 같은 컴포넌트를 각기 다르게 표현할 수 있습니다.

## Named Slot

위에서는 슬롯의 개념을 이해하기 위해 1개의 슬롯만 사용했습니다. 슬롯은 name 속성을 지정하여 여러 개 사용할 수도 있습니다. 좀 전 예제에 네임드 슬롯을 적용해보겠습니다.

```html
<!-- ButtonTab.vue -->
<template>
  <div class="tab panel">
    <!-- 탭 헤더 -->
    <slot name="header"></slot>
    <!-- 탭 본문 -->
    <slot name="content"></slot>
  </div>
</template>


HTML
Copy
<!-- TabContainer.vue -->
<template>
  <button-tab>
    <!-- slot 영역 -->
    <h1 slot="header">First Header</h1>
    <div slot="content" class="content">Tab Contents #1</div>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1 slot="header">Second Header</h1>
    <div slot="content" class="content">Tab Contents #2</div>
  </button-tab>
  <button-tab>
    <!-- slot 영역 -->
    <h1 slot="header">Third Header</h1>
    <div slot="content" class="content">Tab Contents #3</div>
  </button-tab>
</template>
..


HTML
Copy
```

하위 컴포넌트에서 정의한 슬롯 태그 영역에 마크업을 재정의할 때 위와 같이 HTML 표준 태그를 사용하는 방법도 있지만 아래와 같이 `<template>` 태그를 사용할 수도 있습니다.

```html
<button-tab>
  <!-- slot 영역 -->
  <template slot="header">
    <h1>First Header</h1>
  </template>
  <template slot="content">
    <div class="content">Tab Contents #1</div>
  </template>
</button-tab>


HTML
Copy
```

### [범위를 가지는 슬롯](https://kr.vuejs.org/v2/guide/components.html#%EB%B2%94%EC%9C%84%EB%A5%BC-%EA%B0%80%EC%A7%80%EB%8A%94-%EC%8A%AC%EB%A1%AF)

> 2.1.0에 새롭게 추가됨.

범위가 지정된 슬롯은 이미 렌더링 된 엘리먼트 대신 재사용 가능한 템플릿(데이터를 전달할 수 있음)으로 작동하는 특별한 유형의 슬롯입니다.

prop을 컴포넌트에게 전달하는 것처럼, 하위 컴포넌트에서 단순히 데이터를 슬롯에 전달하면 됩니다.

```
<div class="child">
  <slot text="hello from child"></slot>
</div>
```

부모에서, 특별한 속성 `slot-scope`를 가진 `<template>` 엘리먼트가 있어야 합니다. 이것은 범위를 가지는 슬롯을 위한 템플릿임을 나타냅니다. `slot-scope`의 값은 자식으로부터 전달 된 props 객체를 담고있는 임시 변수의 이름입니다:

```
<div class="parent">
  <child>
    <template slot-scope="props">
      <span>hello from parent</span>
      <span>{{ props.text }}</span>
    </template>
  </child>
</div>
```

위를 렌더링하면 출력은 다음과 같습니다.

```
<div class="parent">
  <div class="child">
    <span>hello from parent</span>
    <span>hello from child</span>
  </div>
</div>
```

> 2.5.0+에서, `slot-scope` 는 더이상 `<template>` 뿐 아니라 컴포넌트나 엘리먼트에서도 사용할 수 있습니다.

범위가 지정된 슬롯의 보다 일반적인 사용 사례는 컴포넌트 사용자가 리스트의 각 항목을 렌더링하는 방법을 사용자 정의할 수 있는 리스트 컴포넌트입니다.

```
<my-awesome-list :items="items">
  <!-- scoped slot 역시 이름을 가질 수 있습니다 -->
  <li
    slot="item"
    slot-scope="props"
    class="my-fancy-item">
    {{ props.text }}
  </li>
</my-awesome-list>
```

그리고 리스트 컴포넌트의 템플릿 :

```
<ul>
  <slot name="item"
    v-for="item in items"
    :text="item.text">
    <!-- 대체 컨텐츠는 여기입니다. -->
  </slot>
</ul>
```

#### 디스트럭처링

`slot-scope` 값은 실제로 함수 서명의 인수 위치에 나타날 수 있는 유효한 JavaScript 표현식입니다. 이는 지원되는 환경 (싱글 파일 컴포넌트 또는 최신 브라우저)에서 ES2015 디스트럭처를 사용할 수 있다는 것을 의미합니다.

```
<child>
  <span slot-scope="{ text }">{{ text }}</span>
</child>
```