<template>
  <div class="preloader">
    <div class="spinner"></div>
  </div>
</template>

<script>
export default {
  name: "Preloader",
};
</script>

<style>
.preloader {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f1f7ed;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid rgba(73, 177, 106, 0.3);
  border-top: 4px solid #49b16a;
  border-radius: 50%;
  animation: spin 2s linear infinite;
}

/* Dönme animasyonu */
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
