<template>
  <header>
    <h1>{{ title }}</h1>
  </header>
</template>


<script>

export default {
  name: "Header",
  props: {
    title: String,
  },
  components: {
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
