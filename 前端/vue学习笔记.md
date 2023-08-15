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
- 监视属性
  ```
  Abbreviations
  watch:{
  propertyName:{
  handler(newValue,oldValue){
   //对应属性修改后的动作
  }
  }
  }
  ```
  }
  ```
