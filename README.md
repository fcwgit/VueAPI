# VueAPI
1、创建assets目录、js目录，并引入vue.js

2、创建index.html，增加超链接到vue.directive.html

3、vue.directive.html
引入vue.js
定义div id="app"
创建vue对象
var app = new Vue({
            el:'#app',
            data:{
                message:'HelloWorld',
                num:6
            },
            methods:{
                add:function(){
                    return this.num++;
                }
            }
        })

4、安装live-server
sudo npm install -g live-server
5、启动live-server
live-server

6、事件绑定v-on:click
可以简写为：@click
<button @click="add">add</button>


7、自定义vue组件
Vue.directive("jspang",function(el,binding){
            el.style="color:" + binding.value;
        })

        
 不用加v-，vue自动给加前缀
 <div v-jspang="color">{{num}}</div>