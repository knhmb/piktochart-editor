<template>
  <div class="container">
    <div class="backdrop"></div>
    <div class="loading-spinner-content">
      <div class="loading-spinner"></div>
    </div>
  </div>
</template>

<style scoped>
.container {
  /* width: 100vw; */
  height: 100vh;
  position: fixed;
  top: 0;
  width: 100vw;
  z-index: 98;
  /* height: 100%; */
}

.backdrop {
  background-color: black;
  opacity: 0.3;
  width: 100%;
  height: 100%;
}

.loading-spinner-content {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 99;
}

.loading-spinner {
  width: 2.5rem;
  height: 2.5rem;
  border: 4px solid #ccc;
  border-top-color: #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from {
    transform: rotate(360deg);
  }
}
</style>
