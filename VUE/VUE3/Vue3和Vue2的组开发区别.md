# Vue3和Vue2的组开发区别
### template区别
- Vue3支持碎片，也就是说组件可以拥有多个根节点。而Vue2只支持一个根节点。
  ```
    <template>
      <div></div>
      <div></div>
    </template>
  ```
- Vue2中的template，若template下有多个根节点，一般我们都会使用div包裹元素。🌰：
  ```
    <template>
      <div>
        <h1></h1>
        <span></span>
      </div>
    </template>
  ```
### data区别
- Vue2使用的是选项类型API，而Vue3使用的是合成型API。
- 在Vue2中声明数据
  ```
    export default{
      data(){
        return {
          username:'',
          password:''
        }
      }
    }
  ```
- 在Vue3中声明数据，需要经过三步：
  1. 从vue中引入reactive。
  2. 使用reactive()方法来声明我们的数据为反应性数据。
  3. 使用setup()方法来返回我们的反应性数据，从而我们的template可以获取这些反应性数据。
  

