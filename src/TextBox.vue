<template>
  <div class="textbox">
    <h1>textbox</h1>
    <input type="text" v-model="name" />
    <button @click="next">append input</button>
    <ul>
      <li v-for="(item, index) in list" :key="index">
        {{ item }}
      </li>
    </ul>
    <button @click="toggle('CheckBox')">Goto CheckBox</button>
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
const emit = defineEmits(["rollback", "toggle"]);
const rollback = () => {
  console.log("text box rollback");
  if (list.value.length < 1) {
    emit("rollback");
  } else {
    list.value?.pop();
  }
};
const toggle = action => {
  emit('toggle', action)
}
defineExpose({
  rollback,
});
</script>