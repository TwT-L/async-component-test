<template>
  <div class="parent">
    <a @click="rollback">&lt;back</a>
    <keep-alive>
      <component
        :is="comp[routes[routes?.length - 1]]"
        ref="compRef"
        @rollback="pop"
        @toggle="toggle"
      ></component>
    </keep-alive>
  </div>
</template>

<script setup>
import { ref, defineAsyncComponent, watch, nextTick } from "vue";
import TextBox from "./TextBox.vue";
const CheckBox = defineAsyncComponent(() => import("./CheckBox.vue"));
// import CheckBox from './CheckBox.vue'
const comp = {
  TextBox,
  CheckBox,
};
const routes = ref(["TextBox"]);
const compRef = ref();
const pop = () => {
  if (routes.value.length > 1) {
    routes.value.pop()
  } else {
    alert('already original!')
  }
};
const toggle = action => {
  routes.value.push(action)
}
const rollback = async () => {
  await nextTick()
  console.log('parent do rollback')
  console.log(compRef.value)
  compRef.value?.rollback?.()
}
watch(routes, () => {
  console.log('routes updated', compRef.value)
}, {deep: true})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
