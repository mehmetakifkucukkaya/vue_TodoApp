<template>
  <div id="app">
    <!-- Preloader bileşenini görüntülemek için bir v-if kullanıyoruz -->
    <Preloader v-if="loading" />
    
    <!-- Sayfanın ana içeriği buraya gelir -->
    <div v-else class="container">
      <Header title="Todo App" />
      <AddTaskForm @add-task="addTask" />
      <Tasks
        @updateTask="updateTask($event)"
        @deleteTask="deleteTask($event)"
        :tasks="tasks"
      />
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTaskForm from "./components/AddTaskForm.vue";
import Preloader from "@/components/Preloader.vue"; // Preloader bileşenini ekledik
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTaskForm,
    Preloader, // Preloader bileşenini bileşenler arasına ekledik
  },
  data() {
    return {
      loading: true, // Preloader'ın gösterilip gösterilmeyeceğini belirleyen değişken
      tasks: [],
      APIKey: "65943515998380b98149bdc69a569a8b", //Trello API Key buraya girilmeli
      APIToken:
        "ATTAb2ff5ec830d00d723541530a6f6a158a9050c0c9832caaa0cab3988073e6b527593E1979", //Trello token buraya girilmeli
      idBoard: "64b97f70d4005b477e6fac62", //Trello Board ID'si buraya girilmeli
      idList: "64ba704b58ec7e4fd9025460", // List 1
    };
  },

  methods: {
    // Diğer metodlarınız burada kalıyor

    //Cardları getiren metot.
    getCards() {
      axios
        .get(
          `https://api.trello.com/1/lists/${this.idList}/cards?key=${this.APIKey}&token=${this.APIToken}`
        )
        .then((response) => {
          this.tasks = response.data.map((card) => {
            return {
              id: card.id,
              text: card.name,
              day: card.due,
              priority: card.labels.length > 0 ? card.labels[0].name : "",
              category: card.labels.length > 1 ? card.labels[1].name : "",
            };
          });
          console.log("Tüm kartlar alındı:", this.tasks);

          
          setTimeout(() => {
            this.loading = false;
          }, 2000);
        })
        .catch((error) => {
          console.error("Kartlar alınamadı!:", error);
          this.loading = false; 
        });
    },
  },

  created() {
    this.getCards();
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



 