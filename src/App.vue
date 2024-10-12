<script setup lang="ts">
import { TrashIcon, PencilIcon } from "@heroicons/vue/24/solid";
import { ref } from "vue";
// Import the components
import AddTodoForm from "./components/AddTodoForm.vue"; 
import EditTodo from "./components/EditTodo.vue"; 

export interface Todo {
  id: string;
  title: string;
  description: string;
  completed: boolean;
}

const addOpen = ref<boolean>(false);
const editOpen = ref<boolean>(false);
const editingTodo = ref<Todo>();

let todos = ref<Todo[]>([
  {
    id: crypto.randomUUID(),
    title: "Vue 3",
    description: "Vue 3 is a learn",
    completed: false,
  },
  {
    id: crypto.randomUUID(),
    title: "React JS",
    description: "React js  is a learn",
    completed: false,
  },
  {
    id: crypto.randomUUID(),
    title: "Angular JS",
    description: "Angular js  is a learn",
    completed: false,
  },
]);

function completeTodo(todo: Todo) {
  todo.completed = !todo.completed;
}

function add(todo: Todo) {
  todos.value.push(todo);
}

function delTodo(id: string) {
  const index = todos.value.findIndex((todo) => todo.id === id);
  if (index !== -1) {
    todos.value.splice(index, 1);
  }
}

function editTodo(todo: Todo) {
  editingTodo.value = todo;
  editOpen.value = true;
  console.log(todo);
}

function edit(todo: Pick<Todo, "title" | "description">) {
  const index = todos.value.findIndex(
    (todo) => todo.id === editingTodo.value?.id
  );
  if (index !== -1) {
    todos.value[index].title = todo.title;
    todos.value[index].description = todo.description;
  }
}
</script>

<template>
  <AddTodoForm @add="add" v-if="addOpen" v-model="addOpen" />
  <EditTodo
    :todo="editingTodo"
    @edit="edit"
    v-if="editOpen"
    v-model="editOpen"
  />

  <!--Start todo list-->
  <div class="text-center flex justify-center">
    <button
      @click="addOpen = true"
      class="px-3 py-3 border rounded-md bg-green-500 font-bold text-2xl mt-2 text-white"
      type="button"
    >
      Add Todo
    </button>
  </div>

  <div
    v-for="todo in todos"
    :key="todo.id"
    class="flex mt-2 rounded-md mx-auto justify-between border border-black h-auto w-[600px]"
    :class="[todo.completed ? 'line-through' : '']"
  >
    <div class="p-3 mt-2 rounded-md">
      <h1 class="text-xl font-bold text-black">{{ todo.title }}</h1>
      <p>{{ todo.description }}</p>
    </div>
    <div class="flex items-center gap-1 mr-3">
      <input
        @change="completeTodo(todo)"
        class="cursor-pointer w-[18px] h-[18px]"
        type="checkbox"
      />
      <button @click="editTodo(todo)">
        <PencilIcon class="text-[#1c6363] size-5" />
      </button>
      <button @click="delTodo(todo.id)">
        <TrashIcon class="text-red-600 size-6" />
      </button>
    </div>
  </div>

  <!--End todo list-->
</template>


