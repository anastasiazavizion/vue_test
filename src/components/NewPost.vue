<script setup>
import {ref} from "vue";

const title = ref();
const emits = defineEmits(['add-new-post'])
function addNewPost(){
  emits('add-new-post', title.value)
}
</script>

<template>
<div>
  Titlle: <input v-model="title" type="text">
  <button @click="addNewPost">Add</button>
</div>
</template>

<style scoped>

</style>