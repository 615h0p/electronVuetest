<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Hello, <a-input type="text" placeholder="name here" v-model="name"/>
      </h2>
    </section>
    <section class="create-todos">
      <h3>Create a todo</h3>
      <form @submit.prevent="addTodo">
      <h4>你的下一个目标是？</h4>
      <input type="text" placeholder="打一次篮球~" v-model="input_content">
      <h4>为你的目标选择一个类别吧~</h4>
      <div class="options">
        <label >
          <input type="radio" name="category" id="category1" value="business" v-model="input_category"/>
          <span class="bubble business"></span>
          <div>工作</div>
          <input type="radio" name="category" id="category2" value="persional" v-model="input_category"/>
          <span class="bubble persional"></span>
          <div>生活</div>
        </label>
      </div>
      <input type="submit" value="添加ToDo"/>
      </form>
    </section>
    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div>
        <div v-for="todo in todos_asc" :key="todo.id" :class="`todo-item ${todo.done && 'done'}`">
          <label >
            <input type="checkbox" v-model="todo.done">
            <span :class="`buble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content"/>
          </div>
          <div class="action">
              <button class="delete" @click="removeTodo(todo.createdAt)">删除</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>


import {ref, onMounted, computed, watch} from 'vue'

const todos=ref([])
const todo=ref('')
const name=ref('')
const input_content=ref('')
const input_category=ref(null)
let id=ref(1)
const todos_asc=computed(() => todos.value.slice().sort((a,b)=>{
  return b.createdAt-a.createdAt}))
watch(name, (newval)=>{
  localStorage.setItem('name',newval)
})

watch(todos,newval => {
  localStorage.setItem('todos', JSON.stringify(newval))
},{deep: true})
onMounted(()=>{
  name.value=localStorage.getItem('name')||''
  todos.value=JSON.parse(localStorage.getItem('todos')) || []
})

const removeTodo = (ct)=>{
  todos.value=todos.value.slice().filter(i=>i.createdAt !== ct)
}
const addTodo=()=>{
  if(input_content.value.trim() === '' || input_category.value ===null){
    return
  }
  todos.value.push({
    id: id.value,
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
  id.value=id.value+1
  input_category.value=null
  input_content=''
}
</script>

<style>
#app {
  
}
input {

}
.greeting {
  background-color: aqua;
  display: flex;
  justify-content: center;

}
.create-todos {
  background-color:blue;
}
.options {
  background-color: chartreuse;
}
.todo-list {
  background-color: blueviolet;
}
.todo-item{
  background-color: cornflowerblue;
}
</style>
