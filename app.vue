<template>
  <div
    class="h-screen w-full bg-black flex items-center justify-center bg-teal-lightest font-sans"
  >
    <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-xl">
      <h1 class="text-3xl font-bold mb-4 text-grey-darkest">Todo List</h1>
      <div class="flex mb-4">
        <input
          v-model="inputValue"
          @keyup.enter="clickToAddTodo"
          type="text"
          class="border border-gray-300 grow rounded-l px-4 py-2 focus:outline-none focus:border-blue-500"
          placeholder="Add a todo"
        />
        <button
          v-if="state.isEditing"
          @click="updateTodoList"
          class="bg-teal-500 text-white px-4 py-2 rounded-r"
        >
          Update
        </button>
        <button
          v-else
          @click="clickToAddTodo"
          class="bg-teal-500 text-white px-4 py-2 rounded-r"
        >
          Add
        </button>
      </div>
      <p v-if="todoLists.length === 0" class="text-center mb-2">Empty Todo</p>
      <ul class="bg-white border border-gray-300 border-b-0 rounded">
        <li
          v-for="(todo, index) in todoLists"
          :key="todo.id"
          class="flex items-center justify-between border-b p-4"
        >
          <span>{{ todo.name }}</span>
          <div>
            <button
              @click="editTodoList(index, todo)"
              class="text-green-500 mr-4"
            >
              Edit
            </button>
            <button @click="deleteTodo(index)" class="text-red-500">
              Delete
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from "vue";

export default {
  setup() {
    const inputValue = ref("");
    const selectedIndex = ref(null);
    const state = reactive({
      isEditing: false,
    });

    const todoLists = ref([
      { id: 1, name: "Item 1" },
      { id: 2, name: "Item 2" },
      { id: 3, name: "Item 3" },
    ]);

    function clickToAddTodo() {
      const name = inputValue.value;
      todoLists.value.push({ id: Date.now(), name }); // Access the input value
      console.log({ id: Date.now(), name });
      inputValue.value = ""; // Clear the input value
    }

    function editTodoList(index, todo) {
      inputValue.value = todo.name;
      selectedIndex.value = index;
      state.isEditing = true;
    }

    function updateTodoList() {
      const name = inputValue.value;
      todoLists.value.splice(selectedIndex.value, 1, { name: name });
      state.isEditing = false;
      inputValue.value = "";
    }

    function deleteTodo(index) {
      if (confirm("Are you sure you want to delete?")) {
        todoLists.value.splice(index, 1);
      }
    }

    return {
      inputValue,
      clickToAddTodo,
      todoLists,
      deleteTodo,
      editTodoList,
      state,
      selectedIndex,
      updateTodoList,
    };
  },
};
</script>
