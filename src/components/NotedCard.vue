<template>
  <div
    v-bind:class="['card', { expanded: isCardExpanded }]"
    v-bind:style="{ backgroundColor: note.cards.backgroundColor }"
  >
    <!-- backGroundImage -->
    <div class="card-title-container">
      <p>{{ note.title }}</p>
      <ul class="editBtn">
        <li @click="editNote"><img src="../assets/edit.svg" /></li>
        <li @click="deleteNote"><img src="../assets/trash.svg" /></li>
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
        v-bind:style="{ backgroundColor: note.cards.btnColor }"
      >
        <span v-if="!isCardExpanded">
          <img src="../assets/arrow-down.svg" alt="" />
          Expand
        </span>
        <span v-else>
          <img src="../assets/arrow-up.svg" alt="" /> Collapse</span
        >
      </button>
      <button class="toggle-text-hide-btn" @click="toggleCardHide">
        {{ HideBtnText }}
      </button>
    </div>
    <p class="date">{{ note.cards[0].date.toLocaleString("en-US") }}</p>
  </div>
</template>

<script>
import { ref, defineProps } from "vue";

function toggleCardHide() {
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
<style></style>
