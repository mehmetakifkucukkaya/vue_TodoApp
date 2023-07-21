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
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTaskForm,
  },
  props: {},
  data() {
    return {
      tasks: [],
      APIKey: "65943515998380b98149bdc69a569a8b", //Trello API Key buraya girilmeli
      APIToken:
        "ATTAb2ff5ec830d00d723541530a6f6a158a9050c0c9832caaa0cab3988073e6b527593E1979", //Trello token buraya girilmeli
      idBoard: "64b97f70d4005b477e6fac62", //Trello Board ID'si buraya girilmeli
      idList: "64ba704b58ec7e4fd9025460", // List 1
    };
  },

  methods: {
    addTask(task) {
      this.tasks = [...this.tasks, task]; // Arayüzde ekleme yapar

      // Trelloya task (Card) eklemesi yapar

      const requestBody = {
        name: task.text,
        idList: this.idList, // Doğru liste ID'sini buraya atayın
        key: this.APIKey,
        token: this.APIToken,
      };

      axios
        .post(
          `https://api.trello.com/1/cards?idList=${this.idList}&key=${this.APIKey}&token=${this.APIToken}`,
          requestBody
        )
        .then((response) => {
          console.log("Kart eklendi:", response.data);
          (task.id = response.data["id"]), console.log(task.id);
        })
        .catch((error) => {
          console.error("Kart eklenemedi!:", error);
        });
    },

    deleteTask(id) {
      //Filtreleme işlemi yaparak görevleri siliyoruz (Arayüzden)
      this.tasks = this.tasks.filter((task) => task.id !== id);

      // Trellodan siliyoruz.
      axios
        .delete(
          `https://api.trello.com/1/cards/${id}?key=${this.APIKey}&token=${this.APIToken}`
        )
        .then((response) => {
          console.log("Kart silindi:", response.data);
        })
        .catch((error) => {
          console.error("Kart silinemedi!:", error);
        });
    },
  },

  // Geçici data
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



 