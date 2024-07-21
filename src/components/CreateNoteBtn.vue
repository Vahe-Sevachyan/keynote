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
/* button {
 
  background: linear-gradient(
    to right,
    hsl(173, 68%, 45%),
    hsl(213, 100%, 59%)
  );
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  font-size: 18px;
  border: none;
  margin: 10px;
  padding: 10px 35px;
  text-align: center;
 
  transition: 0.5s;
  background-size: 200% auto;
  color: white;

  border-radius: 10px;
  display: block;
  margin: auto; */
/* background-image: linear-gradient(
    to right,
    #1a2980 0%,
    #26d0ce 51%,
    #1a2980 100%
  ); */
/* box-shadow: 0 0 0px; */
/* margin: 5px; */
/* margin: 5px 0 5px 0; */
/* text-transform: uppercase; 
}*/

button {
  align-self: flex-end;
  background: linear-gradient(
    to right,
    hsl(203, 49%, 63%),
    hsl(213, 100%, 59%)
  );
  border: none;
  border-radius: 4px;
  color: #fff;
  cursor: pointer;
  font-family: "Josefin Sans", sans-serif;
  font-size: 1em;
  padding: 10px 20px;
  background-position: right center; /* change the direction of the change here */
  text-decoration: none;
  /* transition: background 0.5s ease, transform 0.3s ease; */
  transition: 0.5s;
}

button:hover {
  background-position: right center;
  /* transform: scale(1.1); */
}
</style>
