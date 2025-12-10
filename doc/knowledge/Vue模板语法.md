## Vue模板语法

Vue.js 的模板语法是其核心特性之一，它基于 HTML 扩展了一套强大的语法规则，使得开发者能够声明式地将组件实例的数据绑定到最终渲染的 DOM 上。

### 文本插值

数据绑定最基本的形式是使用“Mustache”语法（双大括号）进行文本插值

``` javascript
<template>
  <p>Message: {{ message }}</p>
  <p>计算结果: {{ 5 + 10 }}</p>
  <p>反转字符串: {{ message.split('').reverse().join('') }}</p>
</template>

<script setup>
import { ref } from 'vue';
const message = ref('Hello Vue!');
</script>
```
注意：插值表达式只能包含单个 JavaScript 表达式，不支持语句或流控制

### 指令

指令是带有 v-前缀的特殊属性，其值预期是单个 JavaScript 表达式（v-for和 v-on是例外）。指令的职责是当表达式的值改变时，将其产生的连带影响响应式地作用于 DOM。

- 条件渲染

v-if/ v-else-if/ v-else：根据条件完整地销毁或创建元素
v-show：通过切换 CSS 的 display属性来控制元素显隐，初始渲染开销较大，但切换性能高

``` javascript 
<template>
  <div v-if="type === 'A'">Type A</div>
  <div v-else-if="type === 'B'">Type B</div>
  <div v-else>Other Type</div>
  
  <div v-show="isVisible">这个元素会显示/隐藏</div>
</template>

<script setup>
import { ref } from 'vue';
const type = ref('A');
const isVisible = ref(true);
</script>
```

- 列表渲染 (v-for)

使用 v-for指令基于数组或对象渲染列表。必须为每个项提供一个唯一的 key属性

``` javascript 
<template>
  <!-- 遍历数组 -->
  <ul>
    <li v-for="(item, index) in items" :key="item.id">
      {{ index + 1 }} - {{ item.name }}
    </li>
  </ul>
  
  <!-- 遍历对象 -->
  <div v-for="(value, key, index) in myObject" :key="key">
    {{ index }}. {{ key }}: {{ value }}
  </div>
</template>

<script setup>
import { ref } from 'vue';
const items = ref([
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' }
]);
const myObject = ref({
  title: 'Vue 3 Guide',
  author: 'Vue Team'
});
</script>
```

- 属性绑定 (v-bind)

使用 v-bind指令（缩写为 :）动态设置 HTML 元素的属性

``` javascript

<template>
  <!-- 完整语法 -->
  <img v-bind:src="imageSrc" v-bind:alt="imageAlt">
  <!-- 缩写语法 -->
  <img :src="imageSrc" :alt="imageAlt">
  
  <!-- 动态绑定 Class 与 Style -->
  <div :class="{ active: isActive, 'text-danger': hasError }"></div>
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
</template>

<script setup>
import { ref } from 'vue';
const imageSrc = ref('/path/to/image.jpg');
const imageAlt = ref('An image');
const isActive = ref(true);
const hasError = ref(false);
const activeColor = ref('red');
const fontSize = ref(16);
</script>

```

- 事件处理 (v-on)

使用 v-on指令（缩写为 @）监听 DOM 事件

``` javascript
<template>
  <!-- 完整语法 -->
  <button v-on:click="handleClick">Click me</button>
  <!-- 缩写语法 -->
  <button @click="handleClick">Click me</button>
  
  <!-- 内联事件处理 -->
  <button @click="count++">{{ count }}</button>
  
  <!-- 事件修饰符 -->
  <form @submit.prevent="onSubmit">...</form> <!-- 阻止默认行为 -->
  <button @click.stop="doThis">...</button> <!-- 阻止事件冒泡 -->
  <input @keyup.enter="submit"> <!-- 按键修饰符 -->
</template>

<script setup>
import { ref } from 'vue';
const count = ref(0);
const handleClick = () => {
  alert('Button clicked!');
};
</script>
```


- 双向数据绑定 (v-model)

v-model指令在表单输入元素上创建双向数据绑定

``` javascript
<template>
  <input v-model="message" placeholder="edit me">
  <p>Message is: {{ message }}</p>
  
  <!-- 修饰符 -->
  <input v-model.lazy="message"> <!-- 在 "change" 事件后同步 -->
  <input v-model.number="age" type="number"> <!-- 将输入值转为数字 -->
  <input v-model.trim="message"> <!-- 自动过滤输入的首尾空格 -->
</template>

<script setup>
import { ref } from 'vue';
const message = ref('');
const age = ref(0);
</script>
```

- v-html：更新元素的 innerHTML。注意：只对可信内容使用，绝不要用于用户提交的内容，以防 XSS 攻击
- v-text：更新元素的 textContent。与插值表达式类似
- v-pre：跳过这个元素和它的子元素的编译过程，用来显示原始 Mustache 标签
- v-once：只渲染元素和组件一次，随后的重新渲染被视为静态内容
- v-cloak：这个指令保持在元素上直到关联组件实例编译完毕，可以解决初始化时模板闪烁的问题。通常与 CSS 规则如 [v-cloak] { display: none }一起使用。




### 特别说明：

#### v-on 事件参数传递的几种方式：

1. 默认事件对象传递

当事件处理方法没有显式接收参数时，Vue 会自动传入原生事件对象

``` javascript
<template>
  <button @click="handleClick">按钮1</button>
</template>

<script>
export default {
  methods: {
    handleClick(event) {
      // event 是原生 DOM 事件对象
      console.log(event.target); // 获取触发事件的元素
    }
  }
}
</script>
```

2. 传递自定义参数

需要传递自定义参数时，使用内联函数调用

``` javascript
<template>
  <button @click="handleClick('Hello Vue', 123)">按钮2</button>
</template>

<script>
export default {
  methods: {
    handleClick(message, number) {
      console.log(message); // 'Hello Vue'
      console.log(number);  // 123
    }
  }
}
</script>
```

3. 同时传递事件对象和自定义参数

使用 $event变量显式传递事件对象

``` javascript
<template>
  <button @click="handleClick('自定义参数', $event)">按钮3</button>
</template>

<script>
export default {
  methods: {
    handleClick(customParam, event) {
      console.log(customParam); // '自定义参数'
      console.log(event.target); // 事件对象
    }
  }
}
</script>
```

#### 事件修饰符

事件修饰符是以点开头的特殊后缀，用于常见 DOM 事件处理


| 修饰符      | 用途         | 示例                             |
|----------|------------|--------------------------------|
| .stop    | 阻止事件冒泡     | @click.stop="handleClick"      |
| .prevent | 阻止默认行为     | @submit.prevent="handleSubmit" |
| .capture | 使用事件捕获模式   | @click.capture="handleClick"   |
| .self    | 仅元素自身触发时执行 | @click.self="handleClick"      |
| .once    | 只触发一次      | @click.once="handleClick"      |
| .passive | 优化滚动性能     | @scroll.passive="handleScroll" |

``` javascript
<div id="app">
  <!-- 阻止事件冒泡 -->
  <div @click="divClick">
    <button @click.stop="btnClick">点击我（不会触发divClick）</button>
  </div>
  
  <!-- 阻止默认行为 -->
  <a href="https://baidu.com" @click.prevent="handleLinkClick">去百度</a>
  
  <!-- 修饰符链式调用 -->
  <a href="https://baidu.com" @click.stop.prevent="handleLinkClick">
    去百度（阻止冒泡和默认行为）
  </a>
  
  <!-- 一次性事件 -->
  <button @click.once="handleOnceClick">只能点击一次</button>
</div>
```


#### 按键修饰符

Vue 提供了按键修饰符来处理键盘事件

``` javascript
<!-- 回车键触发 -->
<input @keyup.enter="submitForm">

<!-- 按键别名使用 -->
<input @keyup.enter="submit">      <!-- 回车键 -->
<input @keyup.delete="clearField"> <!-- 删除键 -->
<input @keyup.esc="closeModal">   <!-- ESC键 -->
<input @keyup.space="playVideo">  <!-- 空格键 -->

<!-- 系统修饰键 -->
<input @keyup.ctrl.enter="submit">     <!-- Ctrl + Enter -->
<input @keyup.alt.enter="submit">      <!-- Alt + Enter -->
<button @click.ctrl="doSomething">Ctrl + 点击</button>
```