<template>
  <div
    :class="['card', { expanded: isCardExpanded }]"
    :style="{ backgroundColor: note.cards.backgroundColor }"
  >
    <!-- backgroundImage -->
    <div class="card-title-container">
      <p>{{ note.title }}</p>
      <ul class="editBtn-container">
        <li @click="editNote"><img src="../assets/edit.svg" alt="" /></li>
        <li @click="deleteNote"><img src="../assets/trash.svg" alt="" /></li>
      </ul>
    </div>
    <!--No styles for text-container -->
    <div class="text-container">
      <p v-if="isTextShowing" class="main-text">{{ note.cards.text }}</p>
      <p v-else>...</p>
    </div>
    <div class="btn-container">
      <button
        class="toggle-card-size-btn"
        @click="toggleCardSize"
        :style="{ backgroundColor: note.cards.btnColor }"
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
    <p class="date">{{ note.cards[0].date.toLocaleString("en-US") }}</p>
  </div>
</template>

<script setup>
import { ref, defineProps } from "vue";
const emit = defineEmits("edit-note", "delete-note");
const isCardExpanded = ref(false);
const isTextShowing = ref(false);
const HideBtnText = ref("Show Text");
const props = defineProps({
  note: Object,
});
const note = ref(props.note);
// const HideBtnText = computed(() => (state.showText ? "Hide Text" : "Show Text"));

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

function editNote() {
  emit("edit-note", note.value);
}

function deleteNote() {
  emit("delete-note", note.value);
}
</script>

<style scoped>
/* .cards-container {
  display: flex;
  flex-wrap: wrap;
  width: 1000px;
} */

.toggle-card-size-btn {
  width: 98px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  margin-bottom: 4.5px;
  display: flex;
  justify-content: space-around;
  font-size: 0.8rem;
  /* font-family: "Poppins", sans-serif; */
  /* width: 30%; */
  /* height: 70px; */
  /* padding: 16px; */
  /* display: flex;
  justify-content: center;
  align-items: center;
  text-align: center; */
  /* margin: auto; */
}

.toggle-text-hide-btn {
  width: 80px;
  font-family: "Poppins", sans-serif;
  border-radius: 5px;
  margin-bottom: 4.5px;
  font-size: 13.5px;
}
.btn-container {
  /* display: flex; */
  width: 210px;
  display: flex;
  justify-content: space-evenly;

  /* align-items: center; */
  /* border: 1px solid red; */
}
/* .btn-container button {
  margin: auto;
  border: 1px solid red;
} */
span {
  display: flex;
  align-items: center;
  justify-content: center;
}
.card {
  width: 230px;
  height: 185px;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 15px 15px 15px 0;
  margin: auto;
  /* border: 1px solid white; */
  overflow: hidden;
  /* transition: height 4.3s ease-in-out; */
  margin: 4px 0 4px 0;
}

.card.expanded {
  height: auto;
}

.card-title-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 5px;
  width: 200px;
  font-size: 19px;
  font-family: "Poppins", sans-serif;
  /* height: 30px; */
  /* border: 1px solid black; */
  margin: auto;
  margin-bottom: 5px;
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
  letter-spacing: 0.1px;
  /* overflow-y: auto; */
  border: 2px solid black;
  word-wrap: break-word;
  margin-bottom: 7px;
  margin: auto;
  margin-bottom: 8px;
  /* overflow: hidden; */
  border-radius: 8px;
  padding: 3px;
  font-size: 14px;
  overflow-y: auto;
  /* overflow-x: hidden; */
  transition: height 1.3s ease-in-out;
}
.date {
  font-family: "Nunito", Verdana, sans-serif;
  font-size: 13px;
  text-align: center;
}

.card-title-container p {
  margin-bottom: 5px;
}
.card-title-container span {
  font-size: 20px;
}
.editBtn-container {
  display: flex;
  justify-content: space-between;
  /* border: 1px solid black; */
  list-style: none;
}
.editBtn-container img {
  color: white;
  width: 20px;
  cursor: pointer;
}
</style>
