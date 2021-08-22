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
  ```
    import { reactive } from 'vue'
    
    <script lang="ts">
    import { ref, defineComponent, reactive } from "vue";
    export default defineComponent({
      name: "test",
      setup: () => {
        const state = reactive({
          name: "rose",
        });
        return { state };
      },
    });
    </script>
  ```
  
### methods区别
- vue2是选项型API把methods分割到独立的属性区域，我们可以直接在这个属性里面添加方法处理各种前端逻辑。
  ```
    export default{
      data(){
        return{
          username:''
        }
      },
      methods:{
        login(){
           //...
        }
      }
  ```
- vue3的合成型API里面的setup()方法也是可以用来操控methods的。创建声明方法和数组状态是一样的。
  ```
    <template>
      <div>{{ state.name }}</div> //输出rose
      <div>{{ say() }}</div> //输出 I'm rose
    </template>

    <script lang="ts">
    import { defineComponent, reactive } from "vue";
    export default defineComponent({
      name: "test",
      setup: () => {
        const state = reactive({
          name: "rose",
        });
        const say = () => {
          return `I'm ${state.name}`;
        };
        return { state, say };
      },
    });
    </script>
  ```

### 生命周期
- 在Vue2中，我们可以直接在组件属性中调用Vue的生命周期钩子。
- 在Vue3的合成型API里面的setup()方法可以包含了所有东西，生命周期的钩子就是其中之一，但在Vue3的生命周期钩子不是全局可调用的了，需要另外从vue中引入，和reactive一样引入。
  ```
    import { defineComponent, reactive, onMounted } from "vue";
    export default defineComponent({
      name: "test",
      setup: () => {
        const state = reactive({
          name: "rose",
        });
        const say = () => {
          return `I'm ${state.name}`;
        };
        onMounted(() => {
          console.log(123);
        });
        return { state, say };
      },
    });
  ```

### 计算属性
- 在vue2中实现只需要在组件内的选项属性中添加即可。
- vue3的设计模式就是给予开发者按需引入需要的依赖包，所以若我们要使用computed计算属性，就要在组件内引入computed。
  ```
    import { defineComponent, reactive, computed } from "vue";
    export default defineComponent({
      name: "test",
      setup: () => {
        const state = reactive({
          name: "rose",
          age: computed(() => {
            return 2021 - 1997;
          }),
        });
        const say = () => {
          return `I'm ${state.name}`;
        };
        return { state, say };
      },
    });
  ```

### 侦听属性
- vue2中实现在组件内的选项属性中添加即可。
- vue3中与计算属性一样，需要引入watch。
  ```
    import { defineComponent, reactive, computed, watch, onMounted } from "vue";
    export default defineComponent({
      name: "test",
      setup: () => {
        const state = reactive({
          name: "rose",
          age: computed(() => {
            return 2021 - 1997;
          }),
          x: 0,
        });
        watch(
          () => state.x,
          (newVal, oldVal) => {
            console.log(newVal, oldVal); // 1 0
          }
        );
        const say = () => {
          return `I'm ${state.name}`;
        };
        const change = () => {
          state.x = 1;
        };
        onMounted(() => {
          change();
        });
        return { state, say };
      },
    });
  ```

### 接收Props以及emit
- 在Vue2中，this代表的是当前组件，不是某一个特定的属性，所以我们可以直接使用this访问prop属性。
- 在Vue3中，this无法直接拿到props属性，emit events和组件内的其他属性，不过setuo方法可以接收两个参数：
  1. props -不可变的组件参数
  2. context -Vue3暴露出来的属性(emit、slot、attrs)
  ```
    // 父组件中
      <template>
        <test :msg="state.msg" @change="change" />
      </template>

      <script lang="ts">
      import { defineComponent, reactive } from "vue";
      import test from "./components/test.vue";

      export default defineComponent({
        name: "App",
        components: {
          test,
        },
        setup: () => {
          const state = reactive({
            msg: "123",
          });
          const change = (val: any) => {
            state.msg = val.s;
          };
          return { state, change };
        },
      });
      </script>
    // 子组件
      <template>
        <div>{{ state.name }}</div>
        <div>{{ say() }}</div>
        <div>{{ state.age }}</div>
        <div @click="change">{{ msg }}</div>
      </template>

      <script lang="ts">
      import { defineComponent, reactive, computed } from "vue";
      export default defineComponent({
        name: "test",
        props: {
          msg: {
            type: String,
            default: "",
          },
        },
        emits: ["change"],
        setup: (props, { emit }) => {
          const state = reactive({
            name: "rose",
            age: computed(() => {
              return 2021 - 1997;
            }),
            x: 0,
          });
          const say = () => {
            return `I'm ${state.name}`;
          };
          const change = () => {
            emit("change", { s: "888" });
          };
          return { state, say, change };
        },
      });
      </script>
      
    // 当点击msg时，将会触发函数改变变量 
  ```


