<template>
  <div class="task">
    <h3>
      {{ task.description }}
      <i @click="onDelete(task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>


<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    onDelete(id) {
      this.$emit("deleteTask", id);
    },
  },
};


</script>

<style scope>
.fas {
  color: red;
}

.task {
  background: #c3edc0;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

</style>