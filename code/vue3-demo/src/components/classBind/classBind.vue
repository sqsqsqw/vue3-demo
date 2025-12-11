<template>
    <h3>class样式绑定</h3>

    <h4>绑定对象</h4>

    <div :class="{ 'active':isActive, 'text-danger': hasError, 'a-text': true }">
        一个div
    </div>

    <button @click="changeActive">点击切换大小</button>
    <button @click="changeError">点击切换颜色</button>

    <h4>绑定对象简便方式</h4>

    <div :class="classObject">
        另一个div
    </div>

    <button @click="changeObjActive">点击切换大小</button>
    <button @click="changeObjError">点击切换颜色</button>

    <h4>绑定数组</h4>

    <div :class="classArr">
        另另一个div
    </div>

    <h4>数组和对象</h4>

    <div :class="[{ 'active':isActive }, hasError ? 'text-danger' : '', 'a-text']">
        最后一个div(只能数组嵌套对象)
    </div>
</template>

<script>
    export default {
        data() {
            return {
                isActive: true,
                hasError: false,
                classObject: {
                    'active': true,
                    'text-danger': false, 
                    'a-text': true 
                }
            }
        },
        computed: {
            classArr() {
                return [
                    this.isActive ? 'active' : '', 
                    this.hasError ? 'text-danger' : '', 
                    'a-text'
                ]
            }
        },
        methods: {
            changeActive() {
                this.isActive = !this.isActive
            },
            changeError() {
                this.hasError = !this.hasError
            },
            changeObjActive() {
                this.classObject['active'] = !this.classObject['active']
            },
            changeObjError() {
                this.classObject['text-danger'] = !this.classObject['text-danger']
            },

        }
    }
</script>

<style>
    .text-danger {
        color: red;
    }
    .active {
        font-size: 10px;
    }
    .a-text{
        height: 24px;
    }
</style>