<template>
    <h3>{{ obj.title }}</h3>

    <p>{{ listCounter }}</p>
    <p>{{ listCounterMethod() }}</p>

</template>

<script>
    export default {
        data() {
            return {
                obj: {
                    title: '计算属性',
                    list: [1, 2, 3, 4, 5]
                }
            }
        },
        computed: {
            listCounter() {
                return this.obj.list.length
            }
        },
        methods: {
            listCounterMethod() {
                return this.obj.list.length
            }
        }
    }
</script>