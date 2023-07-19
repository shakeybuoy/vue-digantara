<template>
  <div class="main">
    <img src="/astro.png" alt="astronaut" class="astronaut" />
    <h1>Loading...</h1>
  </div>
</template>
<style scoped>
.main {
  display: grid;
  place-content: center;
  height: 100%;
  width: 100%;
  background-color: #181818;
  gap: 20px;
}
.main h1 {
  margin-top: 50px;
  text-align: center;
  font-weight: 800;
}
@keyframes astronaut {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(50px);
  }
  100% {
    transform: translateY(0px);
  }
}
.astronaut {
  width: 250px;
  height: 300px;
  animation: astronaut 5s linear infinite;
}
</style>
