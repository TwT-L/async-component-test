<template>
  <div class="textbox">
    <h1>checktbox</h1>
    <input type="text" v-model="name" />
    <button @click="next">append input</button>
    <ul>
      <li v-for="(item, index) in list" :key="index">
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, defineExpose } from "vue";
const list = ref([]);
const name = ref();
const next = () => {
  list.value.push(name.value);
  name.value = ''
};
const emit = defineEmits(["rollback"]);
const rollback = () => {
  console.log("checkbox rollback");
  if (list.value.length < 1) {
    emit("rollback");
  } else {
    list.value?.pop();
  }
};
defineExpose({
  rollback,
});
</script>