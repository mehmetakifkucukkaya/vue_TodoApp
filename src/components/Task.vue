<template>
  <div class="task">
    <h3>
      {{ task.text }}
      <i @click="onDelete(task.id)" class="fas fa-times"></i>
    </h3>
    <p>
      {{ task.day }} -
      <span :style="getStyle(task.priority)">{{ task.priority }}</span>
    </p>
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
    getStyle(priority) {
      // Önem derecesine göre renk alıyor
      if (priority === "Düşük") {
        return { color: "blue" };
      } else if (priority === "Orta") {
        return { color: "orange" };
      } else if (priority === "Yüksek") {
        return { color: "#EB0B0B" };
      } else {
        return { color: "black" };
      }
    },
  },
};
</script>

<style scope>
.fas {
  color: red;
}

.task {
  background: #cfe69e;
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