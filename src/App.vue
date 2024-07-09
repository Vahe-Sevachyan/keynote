<script setup></script>

<template>
  <main>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button>+</button>
      </header>
      <div class="cards-container">
        <div class="card">
          <p class="main-text"></p>
          <p class="date"></p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 45px;
}
button {
  border: none;
  padding: 10px;
  width: 30px;
  height: 30px;
  cursor: pointer;
  background-color: black;
  border-radius: 100%;
  color: white;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
