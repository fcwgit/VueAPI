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

8、生命周期

//生命周期方法
        Vue.directive("jspang",{
            bind:function(el,binding){
                console.log('1 - bind');
                el.style = "color:" + binding.value;
            },
            inserted:function(){
                console.log('2 - inserted');
            },
            update:function(){
                console.log('3 - update');
            },
            componentUpdated:function(){
                console.log('4 - componentUpdated');
            },
            unbind:function(){
                console.log('5 - unbind');
            }
        });




绑定标签、id
<author>
    </author>
    <div id="author"></div>
var authorExtend = Vue.extend({
            template:"<p><a :href='authorURL'>{{authorName}}</a></p>",
            data:function(){
                return {
                    authorName:'jspang',
                    authorURL:'http://jspang.com'
                }
            }
        });

        //使用标签的模式
        new authorExtend().$mount("author");
        // 使用id模式
        new authorExtend().$mount("#author");


 ==========================================
 在构造器外面声明数据，然后赋值给构造器
 var outData = {
            count:1,
            // goods:'car'
            arr:['aaa','bbb','ccc']
        }
        var vm = new Vue({
            el:'#app',
            data:outData
        });
通过三种方法修改构造器外面的值
        function add(){
            // vm.count++;
            // Vue.set(outData,'count',5);
            // outData.count++;
            // vm.arr[1] = 'ddd';
            Vue.set(vm.arr,1,'ddd');
        }
vm.arr[1] = 'ddd'; vue无法监听到虚拟DOM 通过数组下标修改数组的情况

Vue.set的用法
Vue.set(vm.arr,1,'ddd');


==============================
beforeCreate:function(){
                console.log('beforeCreate');
            },
            created:function(){
                console.log('created');
            },
            beforeMount:function(){
                console.log('beforeMount');
            },
            mounted:function(){
                console.log('mounted');
            },
            beforeUpdate:function(){
                console.log('beforeUpdate');
            },
            updated:function(){
                console.log('updated');
            }



function destroy(){
            vm.$destroy();
        }
页面销毁



==============================
选项模板
template:`
                <h2 style="color:red">我是选项模板</h2>
            `

标签模板
<template id="dd2">
        <h2 style="color:red">我是template标签模板</h2>
    </template>

var vm = new Vue({
            el:'#app',
            data:{
                message:'helloworld'
            },
            template:'#dd2'


script标签模板
<script type="x-template" id="dd3">
        <h2 style="color:red">我是script标签模板</h2>
    </script>

var vm = new Vue({
            el:'#app',
            data:{
                message:'helloworld'
            },
            template:'#dd3'


==============component1================
一、全局化注册组件
全局化就是在构造器的外部用Vue.component来注册，我们注册现在就注册一个<jspang></jspang>的组件来体验一下。
//注册全局组件
        Vue.component('jspang',{
            template:`<div style="color:red;">全局化注册的jspang标签</div>`
        })
        var app=new Vue({
            el:'#app',
            data:{
            }
        })
<div id="app">
        <jspang></jspang>
    </div>

注意：<jspang></jspang>只能在var app=new Vue({})对应的标签内部  

二、局部注册组件
var app=new Vue({
            el:'#app',
            components:{
                "panda":{
                    template:`<div style="color:red;">局部注册的panda标签</div>`
                }
            }
        })
panda组件只能在 <div id="app"> 内部使用，在组件<div id="ppa"> 内部是无效的

三、组件和指令的区别
组件注册的是一个标签，而指令注册的是已有标签里的一个属性。在实际开发中我们还是用组件比较多，指令用的比较少。因为指令看起来封装的没那么好，这只是个人观点。




==============component2================
定义有属性的组件

定义局部组件
<div id="app">

        <panda here="china">

        </panda>
    </div>
components:{
                "panda":{
                    template:'<div style="color:green">panda from {{here}}</div>',
                    props:['here']
                }
            }
不支持中划线，统一使用驼峰
<panda from-here="sichuan">

        </panda>
components:{
                "panda":{
                    template:'<div style="color:green">panda from {{fromHere}}</div>',
                    props:['fromHere']
                }
            }

注意：建议属性不要带 - 

使用data，项目中常用的方式
var vm = new Vue({
            el:'#app',
            data:{
                message:'China'
            },

需要使用v-bind绑定
<panda v-bind:from-here="message">

        </panda>

v-bind 可以使用 :代替
<panda :from-here="message">

        </panda>


==============component3 父子组件================
子组件要写在前面
// 子组件
        var cityComponent={
            template:`
                <div style="color:green">sichuan of china</div>
            `
        }
        //父组件 ，在父组件中引用子组件
        var pandaComponent ={
            template:`
                <div>
                    <p>panda from chian</p>
                    <city></city>
                </div>
            `,
            components:{
                "city":cityComponent
            }
        }
        
        var vm = new Vue({
            el:'#app',
            components:{
                "panda":pandaComponent
            }
        })
<div id="app">

        <panda></panda>
    </div>









