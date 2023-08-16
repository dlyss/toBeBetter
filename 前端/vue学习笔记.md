- Vue 构建用户界面的渐进式js框架。
- 组件化、声明式编码、虚拟dom+优秀的diff算法
- vue.js分开发版和生成版
- vue-devtools chrome 插件

# 模板语法
- 单项绑定 v-bind，简写为 ：（冒号）
- 双向绑定 v-model，因为是针对value属性，所以可以简写为 v-model
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
