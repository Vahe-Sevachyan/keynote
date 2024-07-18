<template>
  <div>
    <Modal
      v-if="showModal"
      @close="closeModal"
      @add-note="addNote"
      class="modal"
    />
    <EditModal
      v-if="editNote"
      @close="cancelEditModal"
      @updateNote="saveEdit"
      :modifiedNote="selectedNote.text"
      :modifiedTitle="selectedNote.title"
    />
    <!-- @updateTitle="saveTitle" -->
    <div class="container">
      <!-- <button @click="toggleModal()">Create Note</button> -->
      <h1 class="header">Keynote</h1>
      <CreateNoteBtn @create-note-btn="toggleModal()" />

      <div class="card-wrapper">
        <NoteCard
          v-for="(note, index) in notes"
          :key="note.id"
          :note="note"
          @edit-note="editNoteHandler(note)"
          @delete-note="deleteNoteHandler(index)"
          @toggle-card-size="toggleCardSizeHandler(index)"
          @toggle-card-hide="toggleCardHideHandler(index)"
          class="note-card"
        />
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
import Modal from "./components/Modal.vue";
import NoteCard from "./components/NoteCard.vue";
import EditModal from "./components/EditModal.vue";
import CreateNoteBtn from "./components/CreateNoteBtn.vue";
const showModal = ref(false);
const notes = ref([]);
const selectedNote = ref(null);
const editNote = ref(false);
// const selectedTitle = ref(null);
// const updatedNote = ref("");
const isCardExpanded = ref(false);
const isTextShowing = ref(false);
function toggleModal() {
  showModal.value = true;
}
function toggleCardHideHandler() {
  isTextShowing.value = true;
}
//duplicate this function
function toggleCardSizeHandler() {
  isCardExpanded.value = true;
}
// <<<< this logic goes in the addNote function>>>>>>
// const addCategory = () => {
//   if (
//     newCategoryName.value &&
//     !categories.value.some(
//       (category) => category.name === newCategoryName.value
//     )
//   ) {
//     categories.value.push({ name: newCategoryName.value, cards: [] });
//     newCategoryName.value = "";
//   }
// };
// <<<< this logic goes in the addNote function>>>>>>
function addNote(newNote) {
  //   if (notes.includes(newNote.categoryName)) {
  //     notes.value.push();
  //   }
  console.log(notes);
  console.log(newNote);
  notes.value.push(newNote);
  showModal.value = false;
}

function editNoteHandler(note) {
  // Open the modal for editing
  //   console.log(notes);
  editNote.value = true;
  selectedNote.value = note;
}

function saveEdit({ note, title }) {
  if (selectedNote.value) {
    selectedNote.value.text = note;
    selectedNote.value.title = title.replace(/\b\w/g, (char) =>
      char.toUpperCase()
    );
    editNote.value = false;
    selectedNote.value = null;
  }
}

function closeModal() {
  showModal.value = false;
}

function cancelEditModal() {
  editNote.value = false;
  selectedNote.value = null;
}

function deleteNoteHandler(index) {
  if (index !== -1) {
    notes.value.splice(index, 1);
    // editNote.value.splice(index, 1);
  }
  // notes.value.splice(index, 1);
}
</script>

<style scoped>
.modal {
  display: flex;
}
.note-card {
  /* margin: auto; */
}
.header {
  width: 300px;
  margin: auto;
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
  background: linear-gradient(
    to right,
    hsl(170, 56%, 75%),
    hsl(213, 100%, 59%)
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-align: center;
  font-family: monospace;
  font-size: 45px;
}
.container {
  width: 1380px;
  max-height: 800px;
  overflow-y: auto;
  overflow-x: hidden;
  transition: max-height 5.3s ease-in;
  transition: max-height 3.25s ease;
  transition-duration: 2s;
  scrollbar-gutter: stable both-edges;
  /* border: 1px solid red; */
  padding-top: 5px;
  margin: auto;
}

.card-wrapper {
  /* border: 1px solid red; */
  display: flex;
  flex-wrap: wrap;
  margin: auto;
  width: 1290px;
  padding-top: 5px;
  padding-bottom: 5px;
  /* flex-direction: row; */
  /* justify-content: center;
  align-items: center; */
  /* margin: auto; */
  /* border: 1px solid red; */
  /* align-items: center; */
  /* max-height: 800px;
  overflow-y: auto;
  overflow-x: hidden; */
  /* scrollbar-gutter: stable both-edges; */
  /* border: 1px solid red; */
  /* align-items: center; */
  /* flex-wrap: wrap;
  justify-content: flex-start; */
  /* justify-content: flex-start; */
  /* align-items: center; */
}
</style>
