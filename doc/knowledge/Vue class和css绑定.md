## class和css绑定

Vue Class 绑定核心方法速查

| 方法    | 适用场景                | 示例                                            | 优点               |
|-------|---------------------|-----------------------------------------------|------------------|
| 对象语法​ | 根据条件动态切换一个或多个 class | { 'active': isActive, 'error': hasError }     | 直观，条件清晰          |
| 数组语法​ | 应用多个固定的或动态的 class   | ['class-a', isActive ? 'active' : '']         | 灵活组合静态与动态 class  |
| 计算属性​ | 逻辑复杂的 class 绑定      | computedClass() { return { ... } }            | 逻辑封装，模板简洁，可维护性高  |
| 内联样式​ | 需要直接绑定行内样式          | { color: activeColor, fontSize: size + 'px' } | 直接控制样式属性         |

### class各种绑定方式

#### 对象语法

对象语法是最常用的一种方式。对象的键是 class 名称，值是布尔值，当值为 true时，对应的 class 会被应用到元素上

``` javascript
<template>
  <!-- 直接写在模板中 -->
  <div :class="{ 'active': isActive, 'text-danger': hasError }"></div>
  
  <!-- 绑定数据中的对象 -->
  <div :class="classObject"></div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: false,
      // 数据对象形式
      classObject: {
        'active': true,
        'text-danger': false
      }
    };
  }
};
</script>
```

#### 数组语法

数组语法允许你同时应用多个 class，数组中的元素可以是字符串、条件表达式或对象

``` javascript
<template>
  <!-- 应用多个固定 class -->
  <div :class="['base-class', 'title-class']"></div>
  
  <!-- 结合三元表达式进行条件判断 -->
  <div :class="['base-class', isActive ? 'active' : 'inactive']"></div>
  
  <!-- 数组成员也可以是对象 -->
  <div :class="['static-class', { 'conditional-class': someCondition }]"></div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      someCondition: false
    };
  }
};
</script>
```

#### 计算属性

当 class 绑定的逻辑比较复杂时，强烈推荐使用计算属性，它能使模板更简洁、逻辑更清晰

``` javascript
<template>
  <div :class="computedClassObject"></div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      errorType: 'fatal',
      score: 85
    };
  },
  computed: {
    computedClassObject() {
      return {
        'active': this.isActive && this.score > 60,
        'text-danger': this.errorType === 'fatal',
        'highlight': this.score > 90
      };
    }
  }
};
</script>
```

### css绑定方式

#### 对象语法

可以直接在模板中绑定样式对象，Vue 会自动将驼峰命名的属性转换为正确的 CSS 属性名

``` javascript
<template>
  <!-- 直接绑定 -->
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  
  <!-- 绑定样式对象 -->
  <div :style="styleObject"></div>
</template>

<script>
export default {
  data() {
    return {
      activeColor: 'red',
      fontSize: 16,
      // 样式对象形式
      styleObject: {
        color: 'red',
        fontSize: '16px'
      }
    }
  }
}
</script>
```

#### 数组语法

数组语法可以将多个样式对象应用到同一个元素上，后者会覆盖前者的相同属性


``` javascript
<template>
  <div :style="[baseStyles, overridingStyles]"></div>
</template>

<script>
export default {
  data() {
    return {
      baseStyles: {
        color: 'blue',
        fontSize: '14px'
      },
      overridingStyles: {
        fontSize: '18px'  // 将覆盖 baseStyles 的 fontSize
      }
    }
  }
}
</script>
```

#### 自动前缀与多重值

Vue 会自动为需要浏览器前缀的 CSS 属性添加前缀。对于需要多个带前缀值的属性，可以提供数组

``` javascript
<template>
  <!-- Vue 会自动添加适当的前缀 -->
  <div :style="{ transform: rotateValue }"></div>
  
  <!-- 多重值 -->
  <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
</template>
```

### 实用技巧与最佳实践

- 与静态 class 共存

你可以在元素上同时存在静态 class和动态 :class，它们会被自动合并

``` javascript
<div class="static-class" :class="{ 'dynamic-class': isActive }"></div>
```

- 在组件上使用

当你在自定义组件上使用 class绑定时，这些 class 会应用到组件的根元素上，和原生的 class 合并