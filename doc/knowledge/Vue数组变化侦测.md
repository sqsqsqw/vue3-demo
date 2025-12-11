## Vue数组变化侦测

### Vue 2 中的数组变化侦测

Vue 2 使用 Object.defineProperty来追踪对象属性变化，但这种方法无法直接检测数组的变化。因此 Vue 2 采用重写数组原型方法的方案来实现数组响应式
。

重写的数组方法包括：
- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

Vue 2 创建了一个数组方法拦截器，放置在数组实例和 Array.prototype之间

``` javascript
const arrayProto = Array.prototype
const arrayMethods = Object.create(array.prototype)  // 创建拦截器

const methodsToPatch = [
  'push', 'pop', 'shift', 'unshift', 'splice', 'sort', 'reverse'
]

methodsToPatch.forEach(method => {
  const original = arrayProto[method]
  
  Object.defineProperty(arrayMethods, method, {
    value: function mutator(...args) {
      const result = original.apply(this, args)
      const ob = this.__ob__
      
      // 对新增元素进行响应式处理
      let inserted
      if (method === 'push' || method === 'unshift') {
        inserted = args
      } else if (method === 'splice') {
        inserted = args.slice(2)
      }
      if (inserted) ob.observeArray(inserted)
      
      // 通知依赖更新
      ob.dep.notify()
      return result
    },
    enumerable: false,
    writable: true,
    configurable: true
  })
})
```

Vue 2 在 Observer类中为每个响应式数组创建一个依赖管理器（Dep），在 getter 中收集依赖，在数组方法被调用时通过拦截器通知这些依赖进行更新


### Vue 2 中数组侦测的局限性

Vue 2 的数组响应式系统有以下局限性

- 通过索引直接设置元素：vm.items[index] = newValue
- 直接修改数组长度：vm.items.length = newLength

对于上述限制，Vue 2 提供了全局 API 来解决

``` javascript
// 正确设置数组元素
Vue.set(vm.items, index, newValue)
// 或使用实例方法
this.$set(this.items, index, newValue)

// 正确删除数组元素
Vue.delete(vm.items, index)
this.$delete(this.items, index)
```

### Vue 3 中的数组变化侦测

Vue 3 使用 Proxy替代 Object.defineProperty，从根本上解决了数组侦测的局限性

- 可以检测所有类型的数组操作，包括索引操作、长度修改等
- 性能更好，无需重写数组方法
- 支持更多的数据结构类型

Vue 3 提供了 ref和 reactive来处理响应式数组：

``` javascript
import { ref, reactive } from 'vue'

// 使用 ref（推荐基本类型和数组）
const numbers = ref([1, 2, 3])
function addNumber(num) {
  numbers.value.push(num)  // 通过 .value 访问
}

// 使用 reactive（推荐对象和复杂数组）
const state = reactive({
  numbers: [1, 2, 3]
})
function addNumber(num) {
  state.numbers.push(num)  // 直接访问
}
```

### 数组变化监听的实践方法

Vue 提供了多种监听数组变化的方式

- 使用watch监听数组变化

``` javascript
// Vue 2/3 选项式 API
export default {
  data() {
    return {
      items: [1, 2, 3]
    }
  },
  watch: {
    items: {
      handler(newVal, oldVal) {
        console.log('数组发生变化:', newVal)
      },
      deep: true,        // 深度监听元素变化
      immediate: true    // 立即执行一次
    }
  }
}

// Vue 3 组合式 API
import { watch, ref } from 'vue'

const items = ref([1, 2, 3])
watch(items, (newVal, oldVal) => {
  console.log('数组发生变化:', newVal)
}, { deep: true })
```

- 使用计算属性派生数组状态

``` javascript
export default {
  data() {
    return {
      numbers: [1, 2, 3, 4, 5]
    }
  },
  computed: {
    // 过滤偶数
    evenNumbers() {
      return this.numbers.filter(num => num % 2 === 0)
    },
    // 计算总和
    sum() {
      return this.numbers.reduce((total, num) => total + num, 0)
    }
  }
}
```

### 性能优化与最佳实践

深度监听（deep: true）会对数组每个元素进行递归追踪，在大型数组上可能造成性能问题

``` javascript
// 不推荐：对大型数组使用深度监听
watch: {
  largeArray: {
    handler() { /* ... */ },
    deep: true  // 可能造成性能问题
  }
}

// 推荐：使用特定监听或浅监听
watch: {
  'largeArray.length'(newLen) {
    // 只监听长度变化
  },
  'largeArray[0]'(newVal) {
    // 只监听特定元素
  }
}
```

对于大型数组，采用以下优化策略

1. 分页加载：只监听当前页面的数据
2. 虚拟滚动：只渲染可视区域内的元素
3. 防抖监听：避免频繁触发监听回调

``` javascript
import { debounce } from 'lodash-es'

export default {
  data() {
    return {
      largeArray: []  // 大型数据集
    }
  },
  watch: {
    // 使用防抖优化性能
    largeArray: debounce(function(newVal) {
      // 处理逻辑
    }, 300)
  }
}
```
