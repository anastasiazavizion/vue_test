<script setup>
import {computed, provide, ref, watch} from "vue";
import PostData from "./components/PostData.vue";
import NewPost from "./components/NewPost.vue";

const test = '<h1>Test</h1>'
const counter = ref(0);
const name = ref('');

const test2 = computed(()=>{
  return 'Test2 ' + name.value;
})

function increase(amount){
  counter.value += amount;
}

function descrease(){
  if(counter.value > 0){
    counter.value--;
  }
}

watch(name, function (){
  counter.value = 111;
})

function testFunc(){
  alert('enter clicked');
}


const result= ref(0);
function increaseResult(val){
  result.value += val;
}

const resultStr = computed(()=>{
  if(result.value < 37){
    return 'Not yet';
  }
  if(result.value > 37){
    return 'Too much';
  }
  return  result.value;
})

watch(result, function (){
  setTimeout(function (){
    result.value = 0;
  }, 5000)
})

const divs = ref([
  {
    name:'A',
    selected:false
  },
  {
    name:'B',
    selected:false
  }
]);


function selectDiv(name){
  const el = divs.value.find((item)=>item.name === name);
  el.selected = !el.selected;
}

const classVal = ref('');
const paragraphVisible = ref(true);

const classComputed = computed(()=>{
  if(['user1', 'user2'].includes(classVal.value)){
    return classVal.value;
  }
  return  '';
})

function toggleParagraph(){
  paragraphVisible.value = !paragraphVisible.value;
}

const allClasses = computed(()=>{
  return [
    classComputed.value,
    {'hidden': !paragraphVisible.value}
  ]
})

const bgColor = ref('');

const task = ref('');
const tasks= ref([]);
function addTask(){
  if(task.value !== ''){
    tasks.value.push(task.value);
  }
}
const showTasks = ref(true);
const toggleTasksBtnTitle = computed(()=>{
  return showTasks.value ? 'Hide' : 'Show' + 'tasks';
})



const posts = ref([{id:1, title:'Title1', fav:true,}, {id:2, title:'Title2', fav:false}]);

function toggleFav(id, title){
  const post = posts.value.find(p => p.id === id);
  post.fav = !post.fav;
}
function addNewPost(title){

  let newId = 1;
  if(posts.value > 0){
    const maxIdPost = posts.value.reduce(
        (prev, current) => {
          return prev.id > current.id ? prev : current
        }
    );
    newId = maxIdPost.id + 1;
  }

  posts.value.push({
    id:newId,
    title:title,
    fav:false
  })

  console.log(posts.value);
}

function deletePost(id){

  posts.value = posts.value.filter((p)=> p.id !== id);
  //delete posts.value[index];
}

provide('posts',posts);



</script>

<template>
<div>

  <NewPost @add-new-post="addNewPost"></NewPost>
  <div :key="post.title" v-for="post in posts">
    <PostData @delete-post="deletePost" @toggle-fav="toggleFav" :post="post"></PostData>
  </div>


<button @click="increase(2)">Increase</button>
<button @click="descrease">Decrease</button>
  <div>Counter = {{counter}}</div>
  <div v-html="test"></div>
  <div>
    <input @keyup.enter="testFunc" type="text" v-model="name" >
  </div>
  <div v-if="name">
    Your name : {{name}}
  </div>
  <div>
    Test2 = {{test2}}
  </div>


  <div>
    <button @click="increaseResult(5)">Add 5</button>
    <button @click="increaseResult(1)">Add 1</button>
    <p>Result : {{resultStr}}</p>
    <p>Result value: {{result}}</p>
  </div>


  <div>
    <div :class="{
     'borderActive bgGreen': d.selected,
     'borderNotActive': !d.selected,
    }" v-for="d in divs" @click="selectDiv(d.name)">{{d.name}}, selected = {{d.selected}}</div>
  </div>


  <div>
    <input type="text" v-model="classVal">
    <p :class="allClasses">Style me</p>
    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>

  <div>
    <input v-model="bgColor" type="text">
    <p :style="{'backgroundColor': bgColor}">Style me inline!</p>
  </div>
</div>



  <div>
    <input v-model="task" type="text">
    <button @click="addTask">Add task</button>
    <ul v-if="showTasks">
      <li v-for="taskItem in tasks">{{taskItem}}</li>
    </ul>
    <button @click="showTasks = !showTasks">{{toggleTasksBtnTitle}}</button>
  </div>
</template>

<style scoped>

.borderActive{
  border: 1px solid green;
}
.borderNotActive{
  border: 1px solid red;
}

.bgGreen{
  background-color: lightgreen;
}

.user1{
  background-color: purple;
}
.user2{
  background-color: yellow;
}
.hidden{
  display: none;
 }
.block{
  display: block;
}
</style>
