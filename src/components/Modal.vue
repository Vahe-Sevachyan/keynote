<template>
  <div class="over-lay">
    <div class="modal">
      <h2 class="modalTitle">{{ title }}</h2>
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
</script>
<style scoped></style>
