<template>
  <div class="options-container">
    <span @click="handleClick" :class="{'show-options-icon':showOptions} " class="options-icon  text-gray-500">...</span>
    <div v-if="showOptions" id="dropdown"

         class="z-10  bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700">
      <ul class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownDefaultButton">
        <li>
          <a href="#" @click="handleEdit"
             class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Edit</a>
          <a href="#" @click="handleDelete"
             class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Delete</a>
        </li>

      </ul>
    </div>


  </div>
</template>


<style scoped>
.options-container {
  position: relative;
}

#dropdown {
  position: absolute;
  right: 24px;
  transition: all .2s ease-in-out;

}

.options-icon {
  font-family: system-ui;
  font-size: 18px;
  font-weight: bolder;
  transform: rotate(90deg);
  position: absolute;
  right: 8px;
  top: -8px;
  cursor: pointer;
  opacity: 0;
  transition: all .1s ease-in-out;
}

.show-options-icon {
    opacity: 1 !important;

}

.task:hover .options-icon {
  opacity: 1;


}
</style>

<script>
import {defineComponent} from 'vue'


export default defineComponent({
  name: "Options",
  data() {
    return {
      showOptions: false,
    }
  },
  methods: {
    handleClick() {
      this.$root.$emit('close-all-options');
      this.showOptions = !this.showOptions
    },

    handleDelete() {
      this.$emit("delete")
    },

    handleEdit() {
      this.$emit("edit")
    }
  }
})
</script>