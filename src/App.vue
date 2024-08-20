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
    <!-- <Test />
    <Test2 /> -->
    <div class="container">
      <div class="nav-section">
        <h1 class="header">Keynote</h1>
        <CreateNoteBtn
          @create-note-btn="toggleModal()"
          class="create-note-btn"
        />
      </div>
      <p class="punch-line-text">
        Turn messy thoughts into actionable notes. Faster.
      </p>
      <div class="card-wrapper">
        <NoteCard
          v-for="(note, index) in notes"
          :key="note.id"
          :note="note"
          @edit-note="editNoteHandler(note)"
          @delete-note="deleteNoteHandler(index)"
          @toggle-card-size="toggleCardSizeHandler(index)"
          @toggle-card-hide="toggleCardHideHandler(index)"
          @toggle-random-backgroundColor="toggleRandomBackGroundColor(index)"
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
const isCardExpanded = ref(false);
const isTextShowing = ref(false);
const selectedColor = ref("#ffffff");
selectedColor;
function toggleRandomBackGroundColor() {}
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
  notes.value.push(newNote);
  showModal.value = false;
}

function editNoteHandler(note) {
  // Open the modal for editing
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
  const userResponse = confirm(
    `Are you sure you want to delete this note ${notes.value[index].title}?`
  );
  if (userResponse) {
    if (index !== -1) {
      notes.value.splice(index, 1);
      // alert("Note deleted successfully.");
    }
  }
}
</script>

<style scoped>
.nav-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 1000px;
  margin: auto;
  margin-top: 8px;
}
.create-note-btn {
  width: 200px;
  display: flex;
  margin: auto;
}
.modal {
  display: flex;
}

.punch-line-text {
  color: #1d1717;
  width: 260px;
  display: flex;
  font-weight: 900;
  text-align: center;
  margin: auto;
  margin-top: 15px;
  font-size: 20px;
  font-family: "Lexend", sans-serif;
  font-weight: 400;
  padding: 3px;
  background: linear-gradient(
    to right,
    hsl(170, 56%, 75%),
    hsl(213, 100%, 59%)
  );
  border-radius: 5px;
}
.header {
  width: 200px;
  margin: auto;
  display: flex;
  justify-content: center;
  background: linear-gradient(
    to right,
    hsl(170, 56%, 75%),
    hsl(213, 100%, 59%)
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-align: center;
  font-family: "Share Tech Mono", monospace;
  font-size: 50px;
  letter-spacing: 0.1px;
}
.container {
  transition: max-height 5.3s ease-in;
  transition: max-height 3.25s ease;
  transition-duration: 2s;
  scrollbar-gutter: stable both-edges;
  padding-top: 5px;
  margin: auto;

  /* background-size: 150% 100%; */
}

@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.card-wrapper {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row; /* Default setting, items will be added from left to right */
  justify-content: flex-start; /* Default setting, items align to the start (left) */
  margin: auto;
  width: 1240px;
  padding-top: 5px;
  padding-bottom: 5px;
}
</style>
