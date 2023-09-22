## `.sync` 修饰符，子组件修改父组件传来的 `props`

- 父组件

```vue
<!--父组件-->
<template>
  <div>
    <Chlid :prop1.sync="msg"></Chlid>
  </div>
</template>

<script>
  import Child from './Child';

  export default {
    components: { Child },
    data() {
      return {
        msg: 'Hello, Vue!'
      }
    }
  }
</script>
```

- 子组件

```vue
<!--子组件Child.vue-->
<template>
  <div>
    <p>{{ modifiedProp }}</p>
    <button @click="changeMsg">Change Message</button>
  </div>
</template>

<script>
  export default {
    props: {
      prop1: String
    },
    computed: {
      modifiedProp: {
        get() {
          return this.prop1;
        },
        set(newVal) {
          this.$emit('update:prop1', newVal);
        }
      }
    },
    methods: {
      changeMsg() {
        this.modifiedProp = 'Hello, world!';  // 使用计算属性更新prop的值
      }
    }
  }
</script>
```
