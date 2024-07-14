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
    <div class="nav-container">
      <h1>Keynote</h1>
      <!-- <button @click="toggleModal()">Create Note</button> -->
      <CreateNoteBtn @create-note-btn="toggleModal()" />
    </div>
    <!-- @updateTitle="saveTitle" -->
    <div class="notes-container">
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
  margin-bottom: 5px;
}
.nav-container {
  display: flex;
  border: solid 1px red;
  align-items: center;
  width: 570px;
  justify-content: space-between;
  margin: auto;
  background-color: rgb(187, 190, 140);
}
.notes-container {
  background-color: rgb(143, 235, 212);
  display: flex;
  justify-content: center;
  /* align-items: center; */
  margin: auto;
  width: 1480px;
  max-height: 800px;
  overflow-y: auto;
  overflow-x: hidden;
  /* transition: max-height 5.3s ease-in; */
  transition: max-height 3.25s ease;
  transition-duration: 2s;
  scrollbar-gutter: stable both-edges;
  /* border: 1px solid red; */
  padding-top: 5px;
}
.lorem {
  width: 300px;
}
.card-wrapper {
  display: flex;
  flex-direction: column;
  width: 250px;
  justify-content: center;
  align-items: center;
  margin: auto;
  padding-top: 5px;
  padding-bottom: 5px;
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
<!-- // <<<< this logic goes in the addNote function>>>>>>
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
// <<<< this logic goes in the addNote function>>>>>> -->
