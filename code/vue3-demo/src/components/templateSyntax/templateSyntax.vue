<template>
    <h3>模板语法</h3>
    <div>

        <!-- 文本插值 -->
        <div>文本插值：{{ msg }}</div> 
        <hr/>

        <!-- Javascript表达式 -->
        <div>Javascript表达式：{{ msg.split('').reverse().join('') }}</div> 
        <hr/>

        <!-- v-html表达式 -->
        <div>
            <span>v-html表达式：</span>
            <span v-html="htmlContent"></span>
        </div>
        <hr/>

        <!-- v-bind表达式 -->
        <div>
            <div>v-bind表达式：<a v-bind:href="url">Vue.js官方网页</a></div>
            <div>v-bind表达式简化：<a :href="url">Vue.js官方网页</a></div>
            <div>v-bind对象绑定：<a v-bind="objOfAttributes">Vue.js官方网页</a></div>
        </div>
        <hr/>

        <!-- v-for循环 -->
        <div>
            <div>
                <span>v-for循环：</span>
                <span v-for="item in list" style="margin: 0 5px;">{{ item }}</span>
            </div>
            <div>
                <span>携带index的v-for循环：</span>
                <span v-for="(item, index) in list" :key="index" style="margin: 0 5px;">{{ index }}-{{ item }}</span>
            </div>
            <div>
                <span>v-for遍历属性：</span>
                <span v-for="(item, key, index) in userInfo" :key="key" style="margin: 0 5px;">{{ index }}-{{ key }}:{{ item }}</span>
            </div>
        </div>
        <hr/>

        <!-- v-if 判断 -->
         <div>
            <div v-if="isTrue">v-if判断：true</div>
            <div v-else>v-if判断：false</div>
        </div>
        <hr/>

        <!-- v-show 展示 -->
         <div>
            <span>v-show展示：</span>
            <span v-show="isTrue">展示</span>
        </div>
        <hr/>

        <!-- v-on 事件绑定 -->
        <div>
            <div>
                <span>v-on 事件绑定：</span>
                <button v-on:click="isTrue = !isTrue">点击切换</button>
            </div>
            <div>
                <span>v-on 简写：</span>
                <button @click="changeTure">点击切换</button>
            </div>
        </div>
        <hr/>

        <!-- v-model 绑定数据 -->
        <div>
            <div>v-model绑定数据：<input v-model="msg" /></div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                msg: "文本插值",
                htmlContent: "<span>Hello from v-html</span>",
                url: "https://cn.vuejs.org/",
                objOfAttributes: {
                    href: "https://cn.vuejs.org/",
                    target: "_blank" // 在新的浏览器窗口或标签页中打开链接
                },
                list: ['a','b','c','d','f'],
                isTrue: true,
                userInfo: {
                    name: '张三',
                    age: 18, 
                    sex: '男'
                }
            }
        },
        methods:{
            changeTure(){
                this.isTrue = !this.isTrue
            }
        }
    }
</script>