<template>
  <div id="app">
    <Preloader v-if="loading" />
    <div v-else class="container">
      <Header title="Todo App" />
      <AddTaskForm @add-task="addTask" />
      <Tasks @deleteTask="deleteTask($event)" :tasks="tasks" />
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTaskForm from "./components/AddTaskForm.vue";
import Preloader from "@/components/Preloader.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTaskForm,
    Preloader,
  },
  data() {
    return {
      loading: true, // Preloader'ın gösterilip gösterilmeyeceğini belirleyen değişken
      tasks: [],
      APIKey: "", //Trello API Key buraya girilmeli
      APIToken:
        "", //Trello token buraya girilmeli
      idBoard: "", //Trello Board ID'si buraya girilmeli
      idList: "", // Trello'da task eklemek istediğiniz listenin ID'si buraya girilmeli
    };
  },

  methods: {
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

    getLabels() {
      return axios.get(
        `https://api.trello.com/1/boards/${this.idBoard}/labels?key=${this.APIKey}&token=${this.APIToken}`
      );
    },

    addLabelToCard(cardId, labelId) {
      return axios.post(
        `https://api.trello.com/1/cards/${cardId}/idLabels?key=${this.APIKey}&token=${this.APIToken}&value=${labelId}`
      );
    },

    getPriorityLabelId(priority) {
      return axios
        .get(
          `https://api.trello.com/1/boards/${this.idBoard}/labels?key=${this.APIKey}&token=${this.APIToken}`
        )
        .then((response) => {
          const priorityLabel = response.data.find(
            (label) => label.name === priority
          );
          return priorityLabel ? priorityLabel.id : null;
        })
        .catch((error) => {
          console.error("Öncelik etiketleri alınamadı:", error);
          return null;
        });
    },

    getCategoryLabelId(category) {
      return axios
        .get(
          `https://api.trello.com/1/boards/${this.idBoard}/labels?key=${this.APIKey}&token=${this.APIToken}`
        )
        .then((response) => {
          const categoryLabel = response.data.find(
            (label) => label.name === category
          );
          return categoryLabel ? categoryLabel.id : null;
        })
        .catch((error) => {
          console.error("Kategori etiketleri alınamadı:", error);
          return null;
        });
    },

    addTask(task) {
      this.tasks = [...this.tasks, task]; // Arayüzde ekleme yapar

      // Öncelik ve kategori etiketlerini tutmak için boş bir dizi oluşturun
      const labelIds = [];

      // Trelloya task (Card) eklemesi yapmadan önce etiketleri oluşturun
      if (task.priority) {
        this.getPriorityLabelId(task.priority)
          .then((priorityLabelId) => {
            if (priorityLabelId) {
              labelIds.push(priorityLabelId);
            } else {
              console.error(`Öncelik etiketi oluşturulamadı: ${task.priority}`);
            }
          })
          .catch((error) => {
            console.error("Öncelik etiketi alınamadı:", error);
          });
      }

      if (task.category) {
        this.getCategoryLabelId(task.category)
          .then((categoryLabelId) => {
            if (categoryLabelId) {
              labelIds.push(categoryLabelId);
            } else {
              console.error(
                `Kategori etiketi oluşturulamadı: ${task.category}`
              );
            }

            // Etiketleri oluşturduktan sonra, requestBody'e ekleyin
            const requestBody = {
              name: task.text,
              idList: this.idList,
              key: this.APIKey,
              token: this.APIToken,
              due: task.day,
              idLabels: labelIds, // Etiketleri requestBody'e ekleyin
            };

            // Kartı ekleyin
            axios
              .post(
                `https://api.trello.com/1/cards?idList=${this.idList}&key=${this.APIKey}&token=${this.APIToken}`,
                requestBody
              )
              .then((response) => {
                console.log("Kart eklendi:", response.data);
                task.id = response.data["id"];
                console.log("İD:", response.data["id"]);
              })
              .catch((error) => {
                console.error("Kart eklenemedi!:", error);
              });
          })
          .catch((error) => {
            console.error("Kategori etiketi alınamadı:", error);
          });
      }
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



 