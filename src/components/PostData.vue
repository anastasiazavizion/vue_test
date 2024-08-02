<script setup>
import {inject} from "vue";
import PostDataRow from "./PostDataRow.vue";

defineProps({
 post:Object
})
const emits = defineEmits(['toggle-fav', 'delete-post']);

const posts = inject('posts')


function toggleFav(id, title){
  emits('toggle-fav', id, title);
}

function deletePost(id){
  emits('delete-post', id);
}
</script>

<template>
<div>
  {{post.title}}
</div>
  <div v-if="post.fav">Favourite</div>
  <div v-else>Not Favourite</div>
  <div><button @click="toggleFav(post.id, post.title)">Toggle fav</button></div>
  <div><button @click="deletePost(post.id)">Delete</button></div>

  <h1>Posts in PostData:</h1>
  <div v-for="post in posts">
    <h4>{{post.title}}</h4>
  </div>

  <PostDataRow></PostDataRow>


</template>

<style scoped>

</style>