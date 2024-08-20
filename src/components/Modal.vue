<template>
  <div class="overlay">
    <div class="modal">
      <h2 class="modalTitle">{{ title }}</h2>
      <div>
        <input
          type="text"
          class="title-input"
          :placeholder="inputAreaPlaceHolder"
          v-model.trim="titleText"
        />
      </div>
      <textarea
        v-model.trim="noteText"
        name="note"
        id="note"
        cols="30"
        rows="10"
        :placeholder="textAreaPlaceHolder"
      ></textarea>
      <p v-if="errorMessage" class="errorMsg">{{ errorMessage }}</p>
      <button @click="saveNote">{{ saveButtonText }}</button>
      <button id="close" @click="closeModal">{{ closeButtonText }}</button>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";
const titleText = ref("");
const noteText = ref("");
const errorMessage = ref("");
const title = ref("New Note");
const textAreaPlaceHolder = ref("Enter note...");
const inputAreaPlaceHolder = ref("Enter title...");
const saveButtonText = ref("Add Note");
const closeButtonText = ref("Close");
const emit = defineEmits("add-note", "close", "randomBackgroundColor");

function saveNote() {
  if (titleText.value.length < 3) {
    errorMessage.value = "Title must be minimum of 3 characters or more!";
    return;
  } else if (titleText.value.length > 13) {
    errorMessage.value = "Title cant be more than 13 characters";
    return;
  } else if (noteText.value.length < 5) {
    errorMessage.value = "Note must be minimum of 5 characters or more!";
    return;
  }

  const newNote = {
    id: Math.floor(Math.random() * 10000000),
    title: titleText.value.replace(/\b\w/g, (char) => char.toUpperCase()),
    text: noteText.value,
    // date: new Date(),
    date: getNewDate(),
  };
  errorMessage.value = "";
  noteText.value = "";
  titleText.value = "";
  emit("add-note", newNote);
}

function getNewDate() {
  const now = new Date();
  // Extract individual components
  const year = now.getFullYear();
  const month = String(now.getMonth() + 1).padStart(2, "0");
  const day = String(now.getDate()).padStart(2, "0");
  let hours = now.getHours();
  const minutes = String(now.getMinutes()).padStart(2, "0");
  // Determine AM or PM
  const ampm = hours >= 12 ? "PM" : "AM";
  // Convert to 12-hour format
  hours = hours % 12;
  hours = hours ? hours : 12; // If hours is 0, set it to 12
  // Format the date and time
  const formattedDate = `${month}/${day}/${year}`;
  const formattedTime = `${hours}:${minutes} ${ampm}`;
  // Combine date and time
  const displayDateTime = `${formattedDate} ${formattedTime}`;
  return displayDateTime;
}

function closeModal() {
  noteText.value = "";
  titleText.value = "";
  errorMessage.value = "";
  emit("close");
}
</script>

<style scoped>
.title-input {
  margin-bottom: 10px;
  font-size: 17px;
  padding: 4px 0 2px 2px;
  width: 462px;
  font-family: "Josefin Sans", sans-serif;
  background-color: rgb(238, 234, 234);
}
.title-input:focus {
  outline: none;
}

textarea {
  padding: 4px 0 2px 2px;
  font-size: 16px;
  font-family: "Josefin Sans", sans-serif;
  resize: none;
  background-color: rgb(238, 234, 234);
}
textarea:focus {
  outline: none;
}
.overlay {
  position: absolute;
  width: 450px;
  height: 50%;

  z-index: 10px;
  height: auto;
  position: fixed;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.modal {
  background: #3c3c3c;
  border: 1px solid #555;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
  width: 470px;
  padding: 30px;
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.modalTitle {
  margin-top: -15px;
  margin-bottom: 15px;
  text-align: center;
  font-family: "Share Tech Mono", monospace;
  font-size: 2.3rem;
  letter-spacing: -1px;
  background: linear-gradient(
    to right,
    hsl(170, 56%, 75%),
    hsl(213, 100%, 59%)
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
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
  height: 42px;
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
  text-align: center;
  transition: 0.5s;
  background-size: 200% auto;
  font-family: "Josefin Sans", sans-serif;
  font-size: 1.2rem;
  color: white;
  border-radius: 5px;
  display: block;
  width: 100%;
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
</style>
