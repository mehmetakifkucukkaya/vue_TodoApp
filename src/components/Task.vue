<template>
  <div class="task">
    <h3>
      <span class="task-text">{{ task.text }}</span>
      <span class="icons">
        <i @click="onDelete(task.id)" class="fas fa-trash-alt"></i>
        <i @click="onUpdate(task.id)" class="fas fa-edit"></i>
      </span>
    </h3>
    <p>
      <b></b> <b>{{ formatDate(task.day) }}</b>
      <b>&nbsp;&nbsp;&nbsp;&nbsp;</b>
      <b
        ><span :style="getStyle(task.priority)">{{ task.priority }}</span></b
      >
    </p>
    <p>
      <b style="color: grey">{{ task.category }}</b>
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

    onUpdate(id) {
      this.$emit("updateTask", id);
    },
    formatDate(date) {
      if (date) {
        const formattedDate = new Date(date);
        const day = formattedDate.getDate();
        const month = formattedDate.getMonth() + 1;
        const year = formattedDate.getFullYear();
        return `${day < 10 ? "0" : ""}${day}/${
          month < 10 ? "0" : ""
        }${month}/${year}`;
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

.task-text {
  flex: 1; /* Yeni eklenen kısım */
}

.icons {
  display: flex; /* Yeni eklenen kısım */
}

.task i {
  display: inline-block;
  margin-right: 10px;
}

.task i.fa-edit {
  color: blue;
}
</style>