<script setup lang="ts">
import { ref } from "vue";
import { Todo } from "../App.vue";

const emit = defineEmits<{
  (event: "edit", todo: Pick<Todo, "title" | "description">): void;
}>();
const props = defineProps<{
  todo: Todo | undefined;
}>();

const model = defineModel();
const title = ref<string>(props.todo?.title ?? "");
const description = ref<string>(props.todo?.description ?? "");

function editTodo() {
  const todo = {
    title: title.value,
    description: description.value,
  };

  emit("edit", todo);
  model.value = false;
}
function onSubmit() {
  editTodo();
}
</script>


<template>
  <div
    class="absolute w-full backdrop-blur-[2px] flex justify-center items-center h-screen"
    @click.self="model = false"
  >
    <form
      @submit.prevent="onSubmit"
      class="bg-white text-black shadow-xl shadow-gray-400 text-2xl rounded-xl w-[330px] h-[250px]"
    >
      <h3 class="mt-2 ml-2 text-black">Title</h3>
      <input
        class="w-[300px] px-1 text-[15px] rounded-md mt-2 ml-2 bg-gray-300"
        type="text"
        v-model="title"
        required
      />
      <h3 class="mt-2 ml-2">Description</h3>
      <textarea
        class="w-[300px] rounded-md mt-2 ml-2 px-1 text-sm bg-gray-300"
        type="text-sm"
        v-model="description"
      ></textarea>
      <br />
      <button
        class="border w-[300px] rounded-xl text-xl bg-[#006400] mt-2 ml-2 p-1"
        type="submit"
      >
        Add
      </button>
    </form>
  </div>
</template>


