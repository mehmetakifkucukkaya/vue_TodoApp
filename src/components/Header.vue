<template>
  <header>
    <h1>{{ title }}</h1>
    <AddTaskButton text="Görev Ekle" />
  </header>
</template>


<script>
import AddTaskButton from "./AddTaskButton.vue";

export default {
  name: "Header",
  props: {
    title: String,
  },
  components: {
    AddTaskButton,
  },
};
</script>

<style scoped>
header {
  display: flex;
  margin-bottom: 22px;
  justify-content: space-between;
  align-items: center;
}
</style>
