## Vue 选项式 API 与组合式 API 对比

Vue 提供了两种编写组件逻辑的 API 风格：选项式 API​ 和 组合式 API。

- 选项式 API：Vue 2 的传统方式，通过 data、methods、computed等预定义选项组织代码。
- 组合式 API：Vue 3 引入的新范式，基于 setup函数和响应式 API，允许按功能逻辑组织代码。

选项式 API 示例：

```javascript
export default {
  data() {
    return { count: 0, message: 'Hello' }
  },
  computed: {
    reversedMessage() { return this.message.split('').reverse().join('') }
  },
  methods: {
    increment() { this.count++ }
  },
  mounted() { console.log('组件已挂载') }
}
```

组合式 API 示例（使用 \<script setup\>）：

```javascript 
<script setup>
    import { ref, computed, onMounted } from 'vue'

    const count = ref(0)
    const message = ref('Hello')

    const reversedMessage = computed(() => 
        message.value.split('').reverse().join('')
    )

    function increment() { count.value++ }

    onMounted(() => { console.log('组件已挂载') })
</script>
```