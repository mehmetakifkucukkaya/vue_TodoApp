<template>
  <div :key="task.id" v-for="task in tasks">
    <Task @updateTask="$emit('updateTask',task.id)" @deleteTask="$emit('deleteTask',task.id)" :task="task" />
  </div>
</template>


<script>
import Task from "./Task.vue";

export default {
  name: "Tasks",
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
  emits: ["delete-task"],
  methods: {
    deleteTask(id) {
      this.$emit("deleteTask", id);
    },
  },
};
</script>


