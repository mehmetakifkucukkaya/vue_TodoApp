<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Görev</label>
      <input type="text" v-model="text" name="text" placeholder="Görev Girin" />
    </div>

    <div class="form-control">
      <label>Tarih</label>
      <input type="text" v-model="day" name="day" placeholder="Tarih Girin" />
    </div>

    <div>Önem Derecesi</div>
    <select v-model="priority">
      <option disabled>Önem Derecesini Seçiniz</option>
      <option>Yüksek</option>
      <option>Orta</option>
      <option>Düşük</option>
    </select>

    <div>Kategori</div>
    <select v-model="category">
      <option disabled>Kategori Seçiniz</option>
      <option>Genel</option>
      <option>Spor</option>
      <option>Yazılm</option>
      <option>Eğitim</option>
      <option>Eğlence</option>
    </select>

    <input type="submit" value="Görev Ekle" class="btn btn-block" />
  </form>
</template>


<script>
export default {
  name: "AddTaskForm",
  data() {
    return {
      text: "",
      day: "",
      priority: "",
      category: "",
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      // Görev yazılmamşısa uyarı veriyor
      if (!this.text) {
        return alert("Lütfen Görev Ekleyin");
      }

      const newTask = {
        id: Math.floor(Math.random() * 100000),
        text: this.text,
        day: this.day,
        priority: this.priority,
        category: this.category,
      };

      this.$emit("add-task", newTask);

      // Görev verildikten sonra form temizleniyor
      this.text = "";
      this.day = "";
      this.priority = "";
      this.category = "";
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