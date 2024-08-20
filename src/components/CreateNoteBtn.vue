<template>
  <div><button @click="toggleModal()">Create Note</button></div>
</template>
<script setup>
import { ref } from "vue";
const emit = defineEmits("create-note-btn");
const showModal = ref(false);
function toggleModal() {
  showModal.value = true;
  emit("create-note-btn", showModal.value);
}
</script>
<style scoped>
button {
  background-image: linear-gradient(
    to right,
    #24c6dc 0%,
    #514a9d 51%,
    #24c6dc 100%
  );
  width: 100%;
  margin-top: 15px;
  width: 150px;
  height: 40px;
  text-align: center;
  font-family: "Josefin Sans", sans-serif;
  transition: 0.5s;
  background-size: 200% auto;
  font-size: 1.2rem;
  color: white;
  border-radius: 5px;
  border: none;
  display: block;
}

button:hover {
  background-position: right center;
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
</style>
