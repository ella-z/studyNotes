# vue3指令
- 本文主要描述vue3指令与vue2的区别，例如v-model、v-for等等。
### v-for中的Ref数组
- 在Vue2中，在v-for里使用ref属性会用ref数组填充对应的$refs属性。
  ```
     <div :key="index" v-for="(item, index) in 10" :ref="item">{{ item }}</div>
     <script>
        export default {
          mounted() {
            console.log(this.$refs);
          },
        };
     </script>
  ```
  - 输出：
    - ![image](https://user-images.githubusercontent.com/53132020/130347570-5abac102-fd1e-4f99-bf4c-f129c6ed9d4c.png)
  
- 在Vue3中，不再在$ref中自动创建数组，如果我们需要拿到多个refs组成的数组，那么就需要使用函数的方式手动去绑定。
  ```
    <template>
      <div>{{ state.name }}</div>
      <div :key="item" v-for="item in 10" :ref="setItemRefs">{{ item }}</div>
    </template>

    <script lang="ts">
    import { defineComponent, reactive, computed, onMounted } from "vue";
    export default defineComponent({
      name: "test",
      setup() {
        let itemRefs: any[] = [];
        const state = reactive({
          name: "rose",
          age: computed(() => {
            return 2021 - 1997;
          }),
        });
        const setItemRefs = (el: any) => {
          if (el) {
            itemRefs.push(el);
          }
        };
        onMounted(() => {
          console.log(itemRefs);
        });
        return { state, setItemRefs };
      },
    });
    </script>
  ```
  - 输出：
    - ![image](https://user-images.githubusercontent.com/53132020/130348357-31e1972b-5be7-45b9-bd74-ce31547518a1.png)

### 移除v-on.native修饰符
- 在vue2中，v-on的组件的事件监听器只有通过this.$meit才能触发，要将原生DOM监听器添加到子组件的根元素中，可以使用.native修饰符。
  ```
    <Test @click.native="clickTest"> //若不加native，clickTest函数就不会被触发
  ```
- 在vue3中v-on的.native修饰符已经被移除了。同时新增了 emits 选项允许子组件定义真正会被触发的事件。
- 对于子组件中未被定义为组件触发的所有事件监听器，Vue现在将把它们作为原生事件监听器添加到子组件的根元素中(除非在子组件的选项中设置了inheritAttrs: false)，并且子组件中有且仅有一个根节点。
  ```
    //父组件
      <test :msg="state.msg" @change="change" @click="handleNativeClickEvent" />
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
            const handleNativeClickEvent = () => {
              console.log("click");
            };
            return { state, change, handleNativeClickEvent };
          },
        });
      </script>
      
    //子组件
      <template>
        <div>
          <div>{{ state.name }}</div>
          <div>{{ say() }}</div>
          <div>{{ state.age }}</div>
          <div @click="change">{{ msg }}</div>
        </div>
      </template>
  ```

### v-model
- 

### v-if 与 v-for的优先级对比
- 在vue2中，优先级v-for > v-if
- 在vue3中，优先级v-if > v-for
- 由于语法上存在歧义，尽量避免在同一元素上同时使用两者。

