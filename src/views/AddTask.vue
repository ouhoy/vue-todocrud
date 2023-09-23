<template>


  <form class="mt-2.5" @submit.prevent="handleSubmit">
    <div class="relative">

      <input type="search" id="default-search"
             class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
             placeholder="Task of the day..." v-model="newTask">

      <button type="submit"
              class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
        {{ button }}
      </button>
    </div>
    <p v-if="errorMessage" class="mt-2 text-sm text-red-600 dark:text-red-500"><span class="font-bold">Oops!</span>
      {{ errorMessage }}</p>
  </form>


</template>

<script lang="ts">
import {defineComponent} from 'vue'


export default defineComponent({
  name: "AddTask",
  props: { editedTask: Object}
  ,
  data() {
    return {
      newTask: this.editedTask ? this.editedTask.title : "",
      uri: "",
      errorMessage: "",
      button: "Add",
      method: "Add"
    }
  },
  methods: {
    handleSubmit() {


      if (!this.newTask) {
        this.errorMessage = "You need to describe the task first ^^";
        return
      }

      if (this.method === "Add") {
        const createdTask = {title: this.newTask, complete: false, details: ""};

        fetch("http://localhost:3000/projects", {
          method: "POST",
          headers: {"Content-Type": "application/json"},
          body: JSON.stringify(createdTask)
        }).then(() => {

          this.$emit("refresh")
          this.newTask = "";
        }).catch(error => {
          this.errorMessage = "Something went wrong. Please refresh the page and try again.";

          console.error(error)
        })
      }

      if (this.method === "Update") {
        fetch(this.uri, {
          method: "PATCH",
          headers: {"Content-Type": "application/json"},
          body: JSON.stringify({complete: this.editedTask?.complete, title: this.newTask})
        })
            .catch(error => console.error("There is an error: ", error))
            .finally(() => {
              this.newTask = "";
              this.method = "Add";
              this.button = "Add";
              this.$emit("refresh")
              this.$emit("task-update")
            })


      }


    },

  },
  watch: {
    editedTask(newVal, oldVal) {
      this.method = "Update"
      this.newTask = this.editedTask?.title
      this.button = "Update";
      this.uri = "http://localhost:3000/projects/" + this.editedTask?.id;

    }
  }

})
</script>


<style scoped>

</style>