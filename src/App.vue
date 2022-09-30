<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todo = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);

// const todos_asc = computed(() =>
//   todos_asc.value.sort((a, b) => {
//     return b.createdAt - a.createdAt;
//   })
// );

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category === null) {
    return;
  }

  todo.value.push({
    content: input_content.value,
    category: input_category.value,
    done:false,
    createdAt: new Date().getTime(),
  });
};


const removeTodo = todos =>{
  todo.value = todo.value.filter( t => t != todos )
}

watch(todo,newVal=>{
  localStorage.setItem('todo',JSON.stringify(newVal))
},{deep:true})

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todo.value = JSON.parse(localStorage.getItem('todo')|| [])
});

</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up,
        <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your Todo list?</h4>
        <input
          type="text"
          placeholder="e.g. Make a video"
          v-model="input_content"
        />

        <h4>Pick a Category</h4>

        <div class="options">
          <label>
            <input
              type="radio"
              name="'category"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="'category"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3 >TODO LIST</h3>
      <div class="list">

      <div v-for="todo,index in todo" :class="`todo-item ${todo.done && 'done'} `" v-bind:key="index">
        <label>
          <input type="checkbox" class="" v-model="todo.done">
          <span :class="`bubble ${todo.category}`"></span>
        </label>

        <div class="todo-content">
          <input type="text" v-model="todo.content">
        </div>

        <div class="actions">
          <button class="delete" @click="removeTodo(todo)" >Delete</button>
        </div>
      </div>
    </div>
    </section>
  </main>
</template>

<style></style>
