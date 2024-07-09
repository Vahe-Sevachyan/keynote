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
    #1a2980 0%,
    #26d0ce 51%,
    #1a2980 100%
  );
  font-family: Roboto, sans-serif;
  border: none;
  margin: 10px;
  padding: 10px 35px;
  text-align: center;
  text-transform: uppercase;
  transition: 0.5s;
  background-size: 200% auto;
  color: white;
  /* box-shadow: 0 0 0px; */
  border-radius: 10px;
  display: block;
  margin: auto;
  /* margin: 5px; */
  /* margin: 5px 0 5px 0; */
}

button:hover {
  background-position: right center; /* change the direction of the change here */
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
</style>
