<template>
  <div class="over-lay">
    <div class="modal">
      <h2 class="modalTitle">{{ newNoteTitle }}</h2>
      <input
        type="text"
        class="title-input"
        :placeholder="inputAreaPlaceholder"
        v-model.trim="titleText"
      />
      <textarea
        v-model.trim="noteText"
        name="note"
        id="note"
        cols="30"
        rows="10"
        :placeholder="textAreaPlaceholder"
      ></textarea>
      <p v-if="errorMessage" class="errorMsg">{{ errorMessage }}</p>
      <button @click="saveNote">{{ saveButtonText }}</button>
      <button id="close" @click="closeModal">{{ closeButtonText }}</button>
    </div>
  </div>
</template>
<script setup>
import { ref, defineEmits } from "vue";
const noteText = ref("");
const titleText = ref("");
const errorMessage = ref("");
const newNoteTitle = ref("New Note");
const textAreaPlaceholder = ref("Enter note...");
const inputAreaPlaceholder = ref("Enter title");
const saveButtonText = ref("Add Note");
const closeButtonText = ref("Close");
const emit = defineEmits("add-note", "close");

function saveNote() {
  if (titleText.length < 3) {
    errorMessage.value = "Title must be minimum of 3 characters or more!";
    return;
  } else if (noteText.value < 5) {
    errorMessage.value = "Note must be minimum of 5 characters or more!";
  }
  const newNote = {
    id: Math.floor(Math.random() * 1000000),
    title: titleText.value,
    text: noteText.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  };
  errorMessage.value = "";
  noteText.value = "";
  titleText.value = "";
  emit("add-note", newNote);
}
function closeModal() {
  noteText.value = "";
  titleText.value = "";
  errorMessage.value = "";
  emit("close");
}
function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}
</script>
<style scoped>
.title-input {
  border: 1px solid black;
  margin-bottom: 10px;
  width: 100%;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modalTitle {
  margin-top: -15px;
  margin-bottom: 15px;
  text-align: center;
  font-family: "Nunito", Verdana, sans-serif;
  font-size: 25px;
  font-weight: bold;
}
.title-input:focus {
  outline: none;
  border: 2px solid #106de6;
}
.modal {
  width: 450px;
  background-color: #ffffff;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  background-image: linear-gradient(
    to right,
    #24c6dc 0%,
    #514a9d 51%,
    #24c6dc 100%
  );
  width: 100%;
  margin-top: 15px;
  padding: 15px 45px;
  text-align: center;
  text-transform: uppercase;
  transition: 0.5s;
  background-size: 200% auto;
  color: white;
  box-shadow: 0 0 20px #eee;
  border-radius: 10px;
  border: none;
  display: block;
}

.modal button:hover {
  background-position: right center; /* change the direction of the change here */
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}

#close {
  background-image: linear-gradient(
    to right,
    #dc2424 0%,
    #4a569d 51%,
    #dc2424 100%
  );
  margin-top: 15px;
  padding: 15px 45px;
  text-align: center;
  text-transform: uppercase;
  transition: 0.5s;
  background-size: 200% auto;
  color: white;
  box-shadow: 0 0 20px #eee;
  border-radius: 10px;
  display: block;
}

#close:hover {
  background-position: right center; /* change the direction of the change here */
  color: #fff;
  text-decoration: none;
}

.errorMsg {
  color: tomato;
  font-family: Verdana, Tahoma, sans-serif;
}

textarea {
  resize: none;
  border: 1.8px solid black;
  width: 100%;
}
textarea:focus {
  outline: none;
  border: 2px solid #106de6;
}
</style>
