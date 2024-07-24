<template>
  <div class="overlay">
    <div class="modal">
      <h2 class="modalTitle">{{ title }}</h2>
      <div>
        <!-- <input
          type="text"
          class="category-input"
          :placeholder="categoryInputPlaceHolder"
          v-model.trim="categoryText"
        /> -->
        <!-- <select v-model="selectedCategory">
          <option v-for="note in newNote" :value="note.categoryName">
            {{ note.categoryName }}
          </option>
        </select> -->
        <!-- <div>
          <select v-model="selectedCategory">
            <option value="">Select an item</option>
            <option v-for="note in notes" :key="note.id" :value="note.id">
              {{ note.categoryName }}
            </option>
            <option value="new">Create New Category</option>
          </select>
          <div v-if="selectedCategory === 'new'">
            <input
              type="text"
              v-model="newCategoryName"
              placeholder="Enter Category Name"
            />
          </div>
        </div> -->
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
const noteText = ref("");
const selectedCategory = ref("");
const newCategoryName = ref("");
const titleText = ref("");
const categoryText = ref("");
const errorMessage = ref("");
const title = ref("New Note");
const categoryInputPlaceHolder = ref("Enter Category Name");
const textAreaPlaceHolder = ref("Enter note...");
const inputAreaPlaceHolder = ref("Enter title...");
const saveButtonText = ref("Add Note");
const closeButtonText = ref("Close");
const emit = defineEmits("add-note", "close");

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
    date: new Date(),
    backgroundColor: getRandomColor(),
    btnColor: getBtnColor(),
  };
  errorMessage.value = "";
  noteText.value = "";
  titleText.value = "";
  emit("add-note", newNote);
}

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

// const addCard = () => {
//   const categoryIndex = categories.value.findIndex(
//     (category) => category.name === selectedCategory.value
//   );
//   if (categoryIndex !== -1) {
//     categories.value[categoryIndex].cards.push(newCardName.value);
//     newCardName.value = "";
//   }
// };

// function saveNote() {
//   const newNote = {
//     categoryName: newCategoryName.value,
//     id: Math.floor(Math.random() * 10000000),
//     cards: [
//       {
//         id: Math.floor(Math.random() * 10000000),
//         title: titleText.value.replace(/\b\w/g, (char) => char.toUpperCase()),
//         text: noteText.value,
//         date: new Date(),
//         backgroundColor: getRandomColor(),
//         btnColor: getBtnColor(),
//       },
//     ],
//   };
//   // console.log(newNote.cards.date);
//   emit("add-note", newNote);
// }

function closeModal() {
  noteText.value = "";
  titleText.value = "";
  errorMessage.value = "";
  emit("close");
}
//original random color
function getRandomColor() {
  return "hsl(" + Math.random() * 190 + ", 40%, 45%)";

  // return "hsl(" + Math.random() * 250 + ", 50%, 75%)";
}

//Analogous Colors
// function getRandomColor() {
//   return "linear-gradient(135deg, #5e5e5e, #707070, #828282, #949494)"
// }

function getBtnColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

// function getRandomColor() {
//   let angle = Math.floor(Math.random() * 360);
//   let gradient = `linear-gradient(${angle}deg,

//         #${colorCode()},
//         #${colorCode()})`;
//   return gradient;
//   // return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
// }
// function colorCode() {
//   let hexCode1 = "";
//   let hexValues1 = "0123456789abcdef";
//   for (let i = 0; i < 6; i++) {
//     hexCode1 += hexValues1.charAt(
//       Math.floor(Math.random() * hexValues1.length)
//     );
//   }
//   return hexCode1;
// }
</script>

<style scoped>
.title-input {
  /* border: 1px solid black; */
  margin-bottom: 10px;
  font-size: 17px;
  padding: 4px 0 2px 2px;
  width: 462px;
  font-family: "Josefin Sans", sans-serif;
  background-color: rgb(238, 234, 234);
}
.title-input:focus {
  outline: none;
  /* border: 2px solid #106de6; */
}

textarea {
  padding: 4px 0 2px 2px;
  font-size: 16px;
  font-family: "Josefin Sans", sans-serif;
  resize: none;
  background-color: rgb(238, 234, 234);
  /* width: 100%; */
  /* border: 1.8px solid black; */
}
textarea:focus {
  outline: none;
  /* border: 2px solid #106de6; */
}

/* .category-input {
  border: 1px solid black;
  margin-bottom: 10px;
  width: 100%;
}
.category-input:focus {
  outline: none;
  border: 2px solid #106de6;
} */

.overlay {
  position: absolute;
  width: 450px;
  height: 50%;
  /* background-color: rgba(0, 0, 0, 0.77); */
  z-index: 10px;
  height: auto;
  position: fixed;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
  /* margin: auto; */
  /* display: flex;
  align-items: center;
  justify-content: center; */
  /* border: 3px solid red; */

  /* new */
  /* z-index: 1000;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%; */
}
.modal {
  background: #3c3c3c;
  border: 1px solid #555;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
  width: 470px;
  padding: 30px;
  /* position: relative; */
  display: flex;
  flex-direction: column;
  /* new */
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  /* Adjust width and height as needed */
  /* width: 400px;
  height: 200px; */
}

.modalTitle {
  margin-top: -15px;
  margin-bottom: 15px;
  text-align: center;
  font-family: "Share Tech Mono", monospace;
  font-size: 2.3rem;
  letter-spacing: -1px;
  /* color: linear-gradient(to right, hsl(170, 56%, 75%), hsl(213, 100%, 59%)); */
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
  /* text-transform: uppercase; */
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
