<template>
    <h3>css样式绑定</h3>

    <div :style="{color: activeColor, fontSize: fontSize}">一个对象div</div>

    <div :style="[baseStyle, overridingStyles]">一个数组div</div>
</template>

<script>
    export default {
        data() {
            return {
               activeColor: 'red',
               fontSize: '10px',
               baseStyle: {
                   color: 'red',
                   fontSize: '10px'
               },
               overridingStyles: {
                   fontSize: '30px' // 覆盖了baseStyle的fontSize
               }
            }
        },
        computed: {
           
        },
        methods: {

        }
    }
</script>