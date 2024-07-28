<template>
  <div
    :class="['card', { expanded: isCardExpanded }]"
    :style="{ backgroundColor: note.backgroundColor }"
  >
    <!-- backgroundImage -->
    <div class="card-title-container">
      <p class="card-title-text">{{ note.title }}</p>
      <div class="editBtn-container">
        <span @click="editNote"><img src="../assets/edit.svg" alt="" /></span>
        <span @click="deleteNote"
          ><img src="../assets/trash.svg" alt=""
        /></span>
      </div>
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
      <button
        class="toggle-card-size-btn"
        @click="toggleCardSize"
        :style="{ backgroundColor: note.btnColor }"
      >
        <span v-if="!isCardExpanded">
          <img src="../assets/arrow-down.svg" alt="" />
          Expand <font-awesome-icon :icon="['fas', 'arrow-up-from-bracket']" />
        </span>
        <span v-else>
          <img src="../assets/arrow-up.svg" alt="" /> Collapse</span
        >
        <!-- Toggle -->
      </button>
      <button class="toggle-text-hide-btn" @click="toggleCardHide">
        {{ HideBtnText }}
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
const HideBtnText = ref("Show Text");
const valueOfHideBtn = "Show Text";
const props = defineProps({
  note: Object,
});
const note = ref(props.note);

function toggleCardSize() {
  isCardExpanded.value = !isCardExpanded.value;
  emit("toggle-card-size", isCardExpanded.value);
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
  console.log("hello");
  if (HideBtnText.value === "Show Text") {
    isTextShowing.value = true;
  } else if (HideBtnText.value === "Hide Text") {
    return;
  }
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
  margin-top: 60px;
}
.toggle-card-size-btn {
  display: flex;
  justify-content: space-around;
  font-size: 0.8rem;
  /* below  */

  background-image: linear-gradient(
    to right,
    #24c6dc 0%,
    #514a9d 51%,
    #24c6dc 100%
  );
  width: 100%;
  width: 85px;
  height: 25px;
  text-align: center;
  font-family: "Josefin Sans", sans-serif;
  transition: 0.5s;
  background-size: 200% auto;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  border: none;
  display: block;
  margin-bottom: 4.5px;
}

.toggle-text-hide-btn {
  background-image: linear-gradient(
    to right,
    #dc2424 0%,
    #4a569d 51%,
    #dc2424 100%
  );
  width: 100%;
  width: 70px;
  height: 25px;
  text-align: center;
  font-family: "Josefin Sans", sans-serif;
  transition: 0.5s;
  background-size: 200% auto;
  font-size: 0.8rem;
  color: white;
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
.toggle-card-size-btn:hover {
  background-position: right center;
  color: #fff;
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
  height: 260px;
  padding: 5px;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 15px 15px 15px 0;
  margin: auto;
  overflow: hidden;
  margin: 4px 4px 4px 4px;
}

.card.expanded {
  height: auto;
}

.card-title-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 213px;
  margin: auto;
  margin-bottom: 2px;
}
.editBtn-container {
  display: flex;
  justify-content: space-between;
  width: 43px;
  list-style: none;
}

.editBtn-container img {
  color: white;
  width: 20px;
  cursor: pointer;
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
.text-container {
  font-family: "Poppins", Geneva, Tahoma, sans-serif;
  width: 210px;
  height: 300px;
  color: hsl(0, 0%, 0%);
  border: solid rgb(8, 82, 93) 2px;
  letter-spacing: 0.1px;
  word-wrap: break-word;
  margin-bottom: 7px;
  margin: auto;
  margin-bottom: 8px;
  border-radius: 3px;
  padding: 5px;
  overflow-y: auto;
  transition: height 1.3s ease-in-out;
  background-color: hsl(0, 0%, 100%);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.9); /* Box shadow */
  transition: box-shadow 1.3s ease-in-out;
}
.text-container:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2); /* Darker shadow on hover */
}

/* .main-text {
  display: none;
} */
/* .main-text:hover .main-text {
  display: block;
} */

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
