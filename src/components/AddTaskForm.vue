<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Görev</label>
      <input type="text" v-model="text" name="text" placeholder="Görev Girin" />
    </div>
    <h2>
      {{ text }}
    </h2>
    <div class="form-control">
      <label>Tarih</label>
      <input type="text" v-model="day" name="day" placeholder="Tarih Girin" />
    </div>

    <input type="submit" value="Görevi Kaydet" class="btn btn-block" />
  </form>
</template>


<script>
export default {
  name: "AddTask",
  data() {
    return {
      text: "",
      day: "",
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      if (!this.text) {
        alert("Lütfen Görev Ekleyin");
        return;
      }

      const newTask = {
        id: Math.floor(Math.random() * 100000),
        text: this.text,
        day: this.day,
      };

      this.$emit("add-task", newTask);

      this.text = "";
      this.day = "";
    },
  },
};
</script>



<style scoped>
.add-form {
  margin-bottom: 30px;
}

.form-control {
  margin: 16px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
</style>