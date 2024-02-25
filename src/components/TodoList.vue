<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';


interface Todo {
  _id: number;
  text: string;
  done: boolean;
}

let id = ref(0);
const newTodo = ref('');
const todos = ref<Todo[]>([]);


const storageTodo = window.localStorage.getItem( 'todos'); 

if(storageTodo) {
  todos.value = JSON.parse(storageTodo);
}

watch(todos, val => {
  window.localStorage.setItem('todos', JSON.stringify(val));
}, {deep: true});


const addTodos = () => {
  if (!newTodo.value) return;
  todos.value.push({ _id: id.value++, text: newTodo.value, done: false });
  
  newTodo.value = '';
};

const removeTodo = (item: Todo) => {
  todos.value = todos.value.filter((t) => t !== item); 
};

// const toggleTodo = (item: Todo) => {
//   const index = todos.value.indexOf(item);
//   todos.value[index] = { ...item, done: !item.done };
//   saveTodosToLocalStorage();
// }


</script>

<template>
  <h1 class="header">The Todo-List</h1>
  <div class="container">
    <div class="addTodo">
      <form @submit.prevent="addTodos">
        <input type="text" v-model="newTodo" placeholder="Add a todo..." />
        <button>Add</button>
      </form>
      <ul>
        <li v-for="item in todos" :key="item._id">
          <span class="todo-text" :class="{ done: item.done }">{{ item.text }}</span>
          <span class="list-btns">
            <input class="checkbox-custom" type="checkbox" v-model="item.done" >
            <button @click="removeTodo(item)">Remove</button>
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>



<style scoped>



.container {
  display: grid;
  place-items: center;
  padding: 0 20px;
}

.header {
  text-align: center;
  text-decoration: double;
  font-size: 3rem;
  color: #58669f;
}

.addTodo {
  max-width: 100%;
  max-height: 150px;
  
}

form {
  width: 700px;
  display: flex;
  gap: 5px;
  align-items: center;
}

form > input {
  width: 100%;
  max-width: 500px;
  height: 40px;
  text-align: center;
  font-size: 20px;
  color: #58669f;
  text-decoration: underline;
  padding: 10px;
  margin: 10px;
  border: none;
  background-color: #f0f0f0;
  border-radius: 10px;
}

.list-btns {
  display: flex;
  justify-content: flex-end;
  gap: 25px;
}

 form > button {
  width: 100%;
  max-width: 175px;
  padding: 10px;
  margin: 10px;
  border-radius: 25px;
  border: none;
  background-color: #3c98e6;
  color: white;
  cursor: pointer;
  font-weight: bold;
  font-size: 18px;

  
}

input:hover {
  animation: colorChange 5s linear infinite;
}

@keyframes colorChange {
  
  0% {
    background-color: white; 
  }
  50% {
    background-color: lightblue; 
  }
  100% {
    background-color: white;  
  }
}


input:focus {
  outline: 3px solid #58669f;
}

button:active {
  background-color: #488cc8;
}

li {
  list-style: none;
  font-size: 25px;
  color: #58669f;
  font-family:Georgia, 'Times New Roman', Times, serif;
  width: 100%;
  max-width: 700px;
  height: 25px;
  padding: 10px;
  margin: 10px;
  margin-left: -30px;
  margin-right: 30px;
  display: flex;
  justify-content: space-between;
}



li > span > button {
  height: 34px;
  width: 90px;
  border: none;
  border-radius: 10px;
  font-size: large;
  color: #f0f0f0;
  background-color: rgb(210, 78, 78);
  cursor: pointer;
  transition: all .3s ease-in-out;
}

.todo-text {
  display: flex;
  justify-content: space-between;
  gap: 95px;
}

.todo-text > span {
  display: flex;
  justify-content: flex-end;

}


.checkbox-custom {
  margin-top: 5px;
  display: inline-block;
  width: 20px;
  height: 20px;
  background-color: #ddd;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.checkbox-custom:checked {
  background-color: #007bff; /* Change the color when checked */
}

.checkbox-custom::after {
  content: '';
  display: block;
  width: 10px;
  height: 10px;
  margin: 5px;
  border-radius: 2px;
  background-color: white;
  visibility: hidden;
}

.checkbox-custom:checked::after {
  visibility: visible;
}

.input[type="checkbox"] :focus {
  outline: none;
}


@media screen and (max-width: 768px) {
  form > input {
    max-width: 100%; /* Set max-width to 100% for smaller screens */
  }
  
  form > button {
    max-width: 100%; /* Set max-width to 100% for smaller screens */
  }
  
  li {
    max-width: 100%; /* Set max-width to 100% for smaller screens */
  }
}

.todo-text.done {
  text-decoration: line-through;
}

</style>

