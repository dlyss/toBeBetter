- Vue 构建用户界面的渐进式js框架。
- 组件化、声明式编码、虚拟dom+优秀的diff算法
- vue.js分开发版和生成版
- vue-devtools chrome 插件

# 模板语法
- 单项绑定 v-bind:src="xxx"，简写为 ：（冒号）
- 双向绑定 v-model:src="xxx"，因为是针对value属性，所以可以简写为 v-model
- MVVM（model view viewmodel）
- object.defineProperty（obj,propertyName,{value:xxx,enumerable:true}） 对象新加属性
- 数据代理
- 事件绑定 v-on:click="xxx"，简写为@click
- 事件修饰符 @click.prevent;  prevent表达的意思是阻止默认的click行为；类似的有 stop once capture passive
- 键盘事件  @keyup.enter; enter是vue给常用键盘起的别名。类似 esc tab  space right  left
- 计算属性 computed
  ```
  new Vue({
  data:{
  firstName:"fm",
  lastName:"lm"
  },
  computed:{
  fullName:{
  get(){
  return this.firstName+"-"+this.lastName
  },
  set(value):{
  
  }
  }
  }
  }
  )
  ```
  ```
    ///以下是计算属性的简写方式
  computed:{
  fullName(){
  return this.firstName+"-"+this.lastName
  }
  }
  ```
- 监视属性：默认不监视对象内部值的变动，deep=true打开深度监视，可以监视对象内部即多层级属性的变动；
  ```
  Abbreviations
  watch:{
  propertyName:{//正常的写法是 propertyName需要用单引号包起来，简写时可以省略单引号
  handler(newValue,oldValue){
   //对应属性修改后的动作
  }
  }
  }
  ```
  - 绑定class样式： ：class="xxx",以上写法表示该样式为绑定一个动态样式
  - 绑定style样式： ：style = "xxx"
  - 条件渲染 v-show = "false"  或者表达式写法 v-show = "n===1"  当值为true时，显示内容
  - 条件渲染 v-if="n===1" v-else-if = "n===2" 当值为true时，显示内容
    ```
    persons:[
    {id:'001',name:'jack',age:18},
    {id:'002',name:'tom',age:19},
    ]
    ```
  - 列表渲染 v-for = "person in persons" :key="person.id";
  - 列表渲染 v-for = "(p,index) of persons" :key="p.id";
  - [箭头函数](https://mp.weixin.qq.com/s?__biz=MjM5MDA2MTI1MA==&mid=2649134235&idx=3&sn=4c624ee92950ec7233293278524b046d&chksm=be58b336892f3a20caa0d7e01d795d47021c243cfd0bba2e7d3780af03a589ebdc05eeadd3a7&scene=27)
  - 转化为json格式： JSON.stringify(dataDemo)
  - v-model的修饰符：number（只能是数字） lazy（失去焦点再收集数据）  trim（去除空格）
  - [www.boocdn.com](https://www.bootcdn.cn/) js工具库
  - 过滤器
    ```
    <h3>现在是{{time | timeFormater}}</h3>
    filters:{
     timeFormater(value){
     return xxx
    }
    }
    ```
- v-text 向其所在节点渲染文本内容 <h3 v-text="textContent"></h3>
- v-html
- v-cloak
- v-once 所在节点在初次动态渲染后，就视为静态内容
- v-pre
- 自定义指令-函数式:v-big实现
  ```
  directives:{
  big(element,binding){
   element.innerText = binding.value*10
  }
  }
  ```
- 自定义指令-对象式：v-fbind实现
    ```
    <h3 v-fbind="xx"> </h3>
    fbind:{
    bind(element,binding){},//页面渲染时执行
    inserted(element,binding){
    element.focus()
    },//指令所在元素被插入时
    update(element,binding){}//指令所在模板被重新解析时
    
    }
    ```
- 生命周期：当vue加载完元素后，会调用mounted函数
  ```
  new Vue({
  // xxx其他内容
   mounted(){
  setInterval(()=>{
  //xxx逻辑
  },1000)
  }
  })
  ```
  - 组件
    - 非单文件组件：一个文件中包含n 个组件
      ```
      //创建组件
      const school = Vue.extend({
      template:
      '
      <div>
      <h2>{{name}}</h2>
      <h2>{{address}}</h2>
      </div>
      ',
       data(){
      return {
        name:"tom",
        address:"beijing"
      }
      }
      })
      //注册组件
      new Vue({
      el:"#root",
      components:{//局部注册
      xuexiao：school//html页面使用该组件时，直接写： <xuexiao></xuexiao>即可应用该组件
      }
      })
      //全局注册组件
      Vue.component('scholl',scholl)
      ```
      - 组件定义简写
        ```
        //省略了Vue.extend()
        const school = { 
        template:
       '
       <div>
       <h2>{{name}}</h2>
       <h2>{{address}}</h2>
      </div>
      ',
       data(){
      return {
        name:"tom",
        address:"beijing"
      }
      }
      }
    ```
- 组件的嵌套
  ```
  //创建子组件
   const student = Vue.extend({
      template:
      '
      <div>
      <h2>{{name}}</h2>
      <h2>{{age}}</h2>
      </div>
      ',
       data(){
      return {
        name:"tom",
        age:18
      }
      }
      })
        //创建组件
      const school = Vue.extend({
      template:
      '
      <div>
      <h2>{{name}}</h2>
      <h2>{{address}}</h2>
      <student></student>//注意，这里引用了子组件
      </div>
      ',
       data(){
      return {
        name:"tom",
        address:"beijing"
      }
      },
  components:{//组件中注册子组件
    student:student
  }
      })
      //注册组件
      new Vue({
      el:"#root",
      components:{//局部注册
      xuexiao：school//html页面使用该组件时，直接写： <xuexiao></xuexiao>即可应用该组件
      }
      })
      //全局注册组件
      Vue.component('scholl',scholl)
  ```
    - 单文件组件
- Vue脚手架：官方提供的标准化开发工具；框架有隐藏的默认配置可新建vue.config.js，放在根目录下修改。一般不允许修改，类似入库main.js  index.html
- ref标签，类似 id属性，可以通过vue对象得到ref标签对应的元素信息
```
//打标识
<h1 ref="xx">xxx<h1>
//组件中使用
<school ref="xx"></school>
//获取
this.$refs.xxx
```
- props 配置，用于父组件向子组件传递数据，并通过子组件的props属性接收数据 
  ```
  //组件,name age为数据对象中的属性key
  <school name="xxx" age="xxx"></school>

  //vue中声明
  new Vue({

  props:['name','age','xxx']
  })
  //另一种定义类型的写法
  props:{
name:String,
age:Number
  }
  
  ```
- mixin:可以将多个组件公用的配置提取成一个混合对象；
```
//先定义
{
 data(){xxx}
 methods:{xxx}
}
//使用
//全局混入
Vue.mixin(xxx)
//局部混入
mixins:['xxx']
```
- 插件
```
//应用插件xxx
Vue.use(xxx)
```
- 组件样式标签加 scoped，意思是该样式只适用当前组件范围，放在样式名冲突
```
<style scoped>
  xxx
</style>
```
