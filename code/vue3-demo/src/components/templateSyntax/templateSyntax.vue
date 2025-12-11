<template>
    <h1>模板语法</h1>
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
        <hr/>

        <!-- v-on 事件传递方式 -->
        <div>
            <h4>事件传递方式：</h4>
            <div>默认绑定：<button @click="handleClick1">按钮1</button></div>
            <div>自定义参数：<button @click="handleClick2('msg')">按钮2</button></div>
            <div>同时传递：<button @click="handleClick3('msg', $event)">按钮3</button></div>
        </div>
        <hr/>

        <!-- 事件修饰符 -->
        <div>
            <h4>事件修饰符：</h4>
            <div @click="changeTure">
                <button @click.stop="stopClick">点击我（不会触发changeTure）</button>
            </div>
            
            <!-- 阻止默认行为 -->
            <div>
                <a href="https://baidu.com" @click.prevent="changeTure">去百度</a>
            </div>
            
            <!-- 修饰符链式调用 -->
            <div @click="stopClick">
                <a href="https://baidu.com" @click.stop.prevent="changeTure">
                    去百度（阻止冒泡和默认行为）
                </a>
            </div>
            
            <!-- 一次性事件 -->
            <div>
                <button @click.once="changeTure">只能点击一次</button>
            </div>
        </div>
        <hr/>

        <!-- 按键修饰符 -->
        <div>
            <h4>按键修饰符：</h4>
            <!-- 回车键触发 -->
            <input @keyup.enter="keyDown('Enter')" placeholder="press Enter">

            <!-- 按键别名使用 -->
            <input @keyup.delete="keyDown('Delete')" placeholder="press Delete"> <!-- 删除键 -->
            <input @keyup.esc="keyDown('Esc')" placeholder="press Esc">   <!-- ESC键 -->
            <input @keyup.space="keyDown('Space')" placeholder="press Space">  <!-- 空格键 -->

            <!-- 系统修饰键 -->
            <input @keyup.ctrl.enter="keyDown('Ctrl + Enter')" placeholder="press Ctrl + Enter">     <!-- Ctrl + Enter -->
            <input @keyup.alt.enter="keyDown('Alt + Enter')" placeholder="press Alt + Enter">      <!-- Alt + Enter -->
            <button @click.ctrl="keyDown('Ctrl + 点击')">Ctrl + 点击</button>
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
            },
            handleClick1(event){
                console.log(event.target); // 获取触发事件的元素
                this.isTrue = !this.isTrue
            },
            handleClick2(msg){
                console.log(msg);
                this.isTrue = !this.isTrue
            },
            handleClick3(msg, event){
                console.log(msg);
                console.log(event.target);
                this.isTrue = !this.isTrue
            },
            stopClick(){
                console.log("我在stopClick");
            },
            keyDown(keyboard){
                this.msg = keyboard;
            }
        }
    }
</script>