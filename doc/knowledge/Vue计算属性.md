## Vue 计算属性详解

计算属性是 Vue 中一种基于响应式依赖进行缓存的计算结果。当依赖的响应式数据发生变化时，计算属性会自动重新计算，否则直接返回缓存值

Vue 2 选项式 API：

```javascript
export default {
  data() {
    return {
      firstName: '张',
      lastName: '三'
    }
  },
  computed: {
    fullName() {
      return this.firstName + ' ' + this.lastName
    }
  }
}
```

Vue 3 组合式 API：

``` javascript
import { ref, computed } from 'vue'

const firstName = ref('张')
const lastName = ref('三')

const fullName = computed(() => {
  return firstName.value + ' ' + lastName.value
})
```

### 计算属性的核心特性

#### 缓存机制

计算属性会缓存计算结果，只有依赖的响应式数据发生变化时才会重新计算

``` javascript
computed: {
  cachedValue() {
    console.log('计算属性执行了') // 只有依赖变化时才会执行
    return this.someData + 1
  }
}
```

#### 响应式依赖追踪

计算属性自动追踪依赖，当任何依赖项变化时自动更新

``` javascript
computed: {
  complexCalculation() {
    // 自动追踪 this.a, this.b, this.c
    return this.a * this.b + this.c
  }
}
```


### 计算属性 vs 方法 vs 侦听器

``` javascript
export default {
  data() {
    return { count: 0 }
  },
  computed: {
    // 有缓存，count不变时直接返回缓存值
    computedDouble() {
      console.log('计算属性执行')
      return this.count * 2
    }
  },
  methods: {
    // 每次调用都执行
    methodDouble() {
      console.log('方法执行')
      return this.count * 2
    }
  }
}
```