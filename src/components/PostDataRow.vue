<script setup>
import {inject} from "vue";

const posts = inject('posts');
</script>

<template>
  <h1>Posts in PostDataRow:</h1>
  <div v-for="post in posts">
    <h4>{{post.title}}</h4>
  </div>
</template>

<style scoped>

</style>