<template>
  <div
    :class="['card', { expanded: isCardExpanded }]"
    :style="{ background: selectedColor }"
  >
    <!-- backgroundImage -->
    <div class="card-title-container">
      <p class="card-title-text">{{ note.title }}</p>
    </div>
    <!--No styles for text-container -->
    <div class="text-container" @mouseleave="mouseLeave()">
      <p v-if="isTextShowing" class="main-text">{{ note.text }}</p>
      <span v-else class="note-icon">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="24px"
          viewBox="0 -960 960 960"
          width="24px"
          fill="#00000"
        >
          <path
            d="M320-240h320v-80H320v80Zm0-160h320v-80H320v80ZM240-80q-33 0-56.5-23.5T160-160v-640q0-33 23.5-56.5T240-880h320l240 240v480q0 33-23.5 56.5T720-80H240Zm280-520v-200H240v640h480v-440H520ZM240-800v200-200 640-640Z"
          />
        </svg>
        <p
          @mouseenter="
            HideBtnText.value === valueOfHideBtn.value
              ? showNoteOnHover()
              : isTextShowing === false
          "
        >
          Keynote
        </p>
      </span>
    </div>
    <div class="btn-container">
      <!-- toggle size btn -->
      <button class="toggle-card-size-btn" @click="toggleCardSize">
        <span v-if="!isCardExpanded">
          <img src="../assets/down-arrow.svg" alt="" />
        </span>
        <span v-else>
          <img src="../assets/up-arrow.svg" alt="" />
        </span>
        <!-- hide/show btn -->
      </button>
      <button class="toggle-text-hide-btn" @click="toggleCardHide">
        <span v-if="!isTextShowing">
          <img src="../assets/visibility-off.svg" alt="" />
        </span>
        <span v-else>
          <img src="../assets/visibility-on.svg" alt="" />
        </span>
      </button>
      <!-- edit btn -->
      <button @click="editNote" class="edit-note-btn">
        <img src="../assets/edit.svg" alt="" />
      </button>
      <button @click="randomBackgroundColor" class="color-palette-btn">
        <img src="../assets/color-palette.svg" alt="" />
      </button>
      <!-- delete btn -->
      <button @click="deleteNote" class="delete-note-btn">
        <img src="../assets/trash.svg" alt="" />
      </button>
    </div>
    <p class="date">{{ note.date.toLocaleString("en-US") }}</p>
  </div>
</template>

<script setup>
import { ref, defineProps } from "vue";
const emit = defineEmits("edit-note", "delete-note");
const isCardExpanded = ref(false);
const isTextShowing = ref(false);
const valueOfHideBtn = "Show Text";
const HideBtnText = ref("Show Text");
const selectedColor = ref("#0077b6");
const props = defineProps({
  note: Object,
});
const note = ref(props.note);

function toggleCardSize() {
  isCardExpanded.value = !isCardExpanded.value;
  emit("toggle-card-size", isCardExpanded.value);
}

function randomBackgroundColor() {
  selectedColor.value = "hsl(" + Math.random() * 360 + ", 70%, 55%)";
  emit("toggle-random-backgroundColor", selectedColor.value);
}

function toggleCardHide() {
  isTextShowing.value = !isTextShowing.value;
  if (isTextShowing.value === true) {
    HideBtnText.value = "Hide Text";
  } else if (isTextShowing.value === false) {
    HideBtnText.value = "Show Text";
  }
  emit("toggle-card-hide", isTextShowing.value);
}

function showNoteOnHover() {
  setTimeout(() => {
    if (HideBtnText.value === "Show Text") {
      isTextShowing.value = true;
    } else if (HideBtnText.value === "Hide Text") {
      return;
    }
  }, 700);
}

function mouseLeave() {
  if (HideBtnText.value === "Show Text") {
    isTextShowing.value = false;
  }
}

function editNote() {
  emit("edit-note", note.value);
}

function deleteNote() {
  emit("delete-note", note.value);
}
</script>

<style scoped>
.note-icon {
  display: flex;
  justify-content: center;
  flex-direction: column;
}

.toggle-card-size-btn {
  display: flex;
  justify-content: space-around;
  font-size: 0.8rem;
  width: 35px;
  height: 25px;
  text-align: center;
  font-family: "Josefin Sans", sans-serif;
  transition: 0.5s;
  background-size: 200% auto;
  border-radius: 5px;
  cursor: pointer;
  border: none;
  display: block;
  margin-bottom: 4.5px;
}
.color-palette-btn {
  transition: background-color 0.3s ease;
}
.toggle-text-hide-btn {
  width: 35px;
  height: 25px;
  font-family: "Josefin Sans", sans-serif;
  transition: 0.5s;
  background-size: 200% auto;
  border-radius: 5px;
  cursor: pointer;
  border: none;
  display: block;
}
.toggle-text-hide-btn:hover {
  background-position: right center;
  color: #fff;
  cursor: pointer;
}
.edit-note-btn,
.delete-note-btn,
.toggle-text-hide-btn,
.toggle-card-size-btn,
.color-palette-btn {
  width: 35px;
  height: 25px;
  border-radius: 5px;
  border: none;
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;
}
.edit-note-btn:hover,
.toggle-text-hide-btn:hover,
.toggle-card-size-btn:hover,
.color-palette-btn:hover {
  cursor: pointer;
  background-color: #c7ccce;
}
.delete-note-btn:hover {
  background-color: #d00000;
  cursor: pointer;
}
.btn-container {
  width: 210px;
  display: flex;
  justify-content: space-evenly;
  margin: auto;
}
span {
  display: flex;
  align-items: center;
  justify-content: center;
}
.card {
  width: 230px;
  height: 135px;
  padding: 5px;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 15px 15px 15px 0;
  margin: auto;
  overflow: hidden;
  margin: 4px 4px 4px 4px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
}
.card.expanded {
  height: auto;
}
.card-title-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 215px;
  margin: auto;
  margin-bottom: 5px;
  padding: 2px;
  border: 1 px solid black;
  border-radius: 2px;
}
.card-title-container,
.text-container {
  background-color: #f0f0f0;
}
.text-container {
  font-family: "Share Tech Mono", monospace;
  width: 210px;
  height: 300px;
  letter-spacing: 0.1px;
  word-wrap: break-word;
  margin-bottom: 7px;
  margin: auto;
  margin-bottom: 8px;
  border-radius: 3px;
  padding: 5px;
  overflow-y: auto;
  transition: height 1.3s ease-in-out;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Box shadow */
  transition: box-shadow 0.4s ease-in-out;
  color: black;
}

.text-container:hover {
  box-shadow: 0 8px 16px rgba(0, 1, 1, 3.2);
}
.main-text::-webkit-scrollbar {
  width: 12px; /* Adjust scrollbar width */
}

.main-text::-webkit-scrollbar-thumb {
  background-color: #b82020; /* Adjust thumb color */
  border-radius: 6px; /* Adjust thumb border radius */
}

.main-text::-webkit-scrollbar-track {
  background-color: #eee; /* Adjust track color */
}
@keyframes text-container {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(-100%, -100%);
  }
}

.date {
  font-family: "Share Tech Mono", monospace;
  font-size: 13px;
  text-align: center;
  margin: auto;
}

.card-title-text {
  font-size: 1.1rem;
  font-family: "Share Tech Mono", monospace;
  color: hsl(0, 0%, 0%);
}
.card-title-container span {
  font-size: 20px;
}
</style>
