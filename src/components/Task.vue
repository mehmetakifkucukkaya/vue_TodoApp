<template>
  <div class="task">
    <h3>
      {{ task.text }}
      <i @click="onDelete(task.id)" class="fas fa-times"></i>
    </h3>
    <p>
      <b>Date:</b> {{ formatDate(task.day) }}
      <b> <span :style="getStyle(task.priority)">{{ task.priority }}</span> </b> 
    </p>
    <p>
      <b>Category:</b> {{ task.category }}
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
        
    formatDate(date) {
      if (date) {
        const formattedDate = new Date(date);
        const day = formattedDate.getDate();
        const month = formattedDate.getMonth() + 1;
        const year = formattedDate.getFullYear();
        return `${day < 10 ? "0" : ""}${day}/${month < 10 ? "0" : ""}${month}/${year}`;
      }
      return "";
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