<template>
  <div class="container">
    <Header title="Todo App" />
    <AddTaskForm @add-task="addTask" />
    <Tasks @deleteTask="deleteTask($event)" :tasks="tasks" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTaskForm from "./components/AddTaskForm.vue";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTaskForm,
  },
  props: {
    APIKey: "65943515998380b98149bdc69a569a8b",
    APIToken:
      "ATTAb2ff5ec830d00d723541530a6f6a158a9050c0c9832caaa0cab3988073e6b527593E1979",
  },
  data() {
    return {
      tasks: [],
    };
  },

  methods: {
    addTask(task) {
      this.tasks = [...this.tasks, task];
    },
    deleteTask(id) {
      //Filtreleme işlemi yaparak görevleri siliyoruz
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
  },

  created() {
    this.tasks = [
      {
        id: 1,
        text: "İngilizce Çalış",
        day: "21 Temmuz ",
        priority: "Düşük",
        category: "Eğitim",
      },
      {
        id: 2,
        text: "Algoritma Çalış",
        day: "22 Temmuz ",
        priority: "Düşük",
        category: "Yazılım",
      },
      {
        id: 3,
        text: "Python Çalış",
        day: "23 Temmuz ",
        priority: "Orta",
        category: "Yazılım",
      },
      {
        id: 4,
        text: "Kitap Oku",
        day: "24 Temmuz ",
        priority: "Yüksek",
        category: "Genel",
      },
    ];
  },
};
</script>



<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid slategrey;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #49b16a;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
