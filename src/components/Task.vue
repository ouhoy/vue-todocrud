<template>
  <li :key="task?.id"
      :class="{'bg-gray-50': isSelected}"
      class=" task w-full  flex items-center justify-between  border-b border-gray-200 rounded-t-lg dark:border-gray-600">
    <div class="  flex items-center pl-3">
      <input @click="handleCheck" :id="task?.id" type="checkbox" :checked="task?.complete"
             class=" cursor-pointer w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-700 dark:focus:ring-offset-gray-700 focus:ring-2 dark:bg-gray-600 dark:border-gray-500">
      <label
          class="  w-full py-3 ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">{{
          task.title
        }}</label>
    </div>
    <Options @delete="handleDelete" @cancel="handleCancel" :isSelected="isSelected" @edit="handleEdit" :key="task?.id"/>
  </li>
</template>

<script lang="ts">
import {defineComponent} from 'vue'
import Options from "@/components/Options.vue";

interface Task {
  id: number,
  title: string,
  details: string
  complete: boolean,
}

export default defineComponent({
  name: "Task",
  components: {Options},

  props: {task: Object as () => Task},
  data() {
    return {
      uri: "http://localhost:3000/projects/" + this.task?.id,
      isSelected: false,
    }
  }
  ,
  methods: {
    handleDelete() {
      fetch(this.uri, {method: "DELETE"})
          .then(() => this.$emit("delete", this.task?.id))
          .catch(error => console.error("There is an error: ", error))
    },
    handleCheck() {
      fetch(this.uri, {
        method: "PATCH",
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({complete: !this.task?.complete})
      })
          .catch(error => console.error("There is an error: ", error))
    },
    handleCancel() {
      this.isSelected = false

      this.$emit("cancel")
    }
    ,

    handleEdit() {
      this.isSelected = true;
      this.$emit("edit", this.task?.id)
    },

  },

  watch: {
    task(oldVal, newVal) {
      this.isSelected = false
    }
  },


})
</script>


<style scoped>


li:last-child {
  border-bottom: 0 !important;
}


</style>
