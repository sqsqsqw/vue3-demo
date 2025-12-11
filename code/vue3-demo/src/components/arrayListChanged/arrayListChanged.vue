<template>
    <h1>数组变化侦测</h1>

    <div>
        <div>
            <div>数组变化侦测：</div>
            <div>
                <button @click="add">添加</button>
                <button @click="remove">删除</button>
                <button @click="change">修改</button>
            </div>
        </div>
        <div>
            <div>数组变化侦测结果：</div>
            <ul>
                <li v-for="item in list">{{ item }}</li>
            </ul>
        </div>
    </div>

    <hr/>

    <button @click="concatHandle">合并数组</button>
    <h3>数组1</h3>
    <p v-for="value in nums1" :key="value">{{ value }}</p>
    <h3>数组2</h3>
    <p v-for="value in nums2" :key="value">{{ value }}</p>

</template>

<script>
    export default {
        data() {
            return {
                list: [1, 2, 3, 4, 5],
                nums1: [1, 2, 3, 4, 5],
                nums2: [6, 7, 8, 9, 10]
            }
        },
        methods: {
            add() {
                this.list.push(6)
            },
            remove() {
                this.list.pop()
            },
            change() {
                // vue2 中，数组的修改不会触发视图更新
                this.list[0] = 100

                // vue2的写法是这样的
                // this.$set(this.list, 0, 100)
                // 或者
                // Vue.set(this.list, 0, 100)
            },
            concatHandle(){
                this.nums1 = this.nums1.concat(this.nums2)
            }
        }
    }
</script>