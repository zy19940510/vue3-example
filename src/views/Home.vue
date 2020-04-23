<template>
  <div>count: {{ count }}</div>
  <div>doubleCount: {{ doubleCount }}</div>
  <div>obj.foo: {{ obj.foo }}</div>
  <div>obj.foo: {{ foo }}</div>
  <button @click="add">add</button>
  <div>
    <span v-for="item in arr" :key="item">{{ item }}</span>
  </div>
</template>

<script>
import { ref, onMounted, reactive, computed, watch, toRefs } from "vue";
export default {
  name: "Home",
  // 1.代替了data、beforeCreate、created
  // 2.所有生命周期都写在里边
  // 3.oop -> fp, 取消this, 所需上下文都会通过参数的形势传进来
  // 4.可以return包含状态、事件的对象，或者tempalte、jsx
  setup(props, context) {
    onMounted(() => {
      console.log("mounted");
    });

    console.log(props, context);

    // 5.用什么来决定用ref还是reactive?
    const count = ref(6);
    const title = ref("");
    const isValid = ref(true);
    const foo = ref(null);
    const arr = ref(["🍊", "🍌", "🍎", "🍔"]);
    const obj = reactive({ foo: "bar" });
    console.log("obj:", obj);
    // 6.为什么需要.value来拿到值，而不是直接用count来表示?
    // 7.为什么在这里需要用.value，在template里不需要?
    console.log("count:", count, "value:", count.value);

    // 自定义事件
    const add = () => count.value++;

    // computed用法，和vue2差不多
    const doubleCount = computed(() => count.value * 2);

    // watch用法
    watch(
      () => count.value,
      (newv, oldv) => {
        console.log(newv, oldv);
      },
      { immediate: true, deep: true }
    );
    // 可同时监听多个值的变化!不用再像vue2一样需要先写computed -> return foo && count, 再watch
    watch([() => count.value, () => obj.foo], ([foo, count]) => {
      console.log("foo", foo);
      console.log("count", count);
    });

    return {
      count,
      obj,
      arr,
      doubleCount,
      add,
      ...toRefs(obj),
    };
  },
};
</script>
