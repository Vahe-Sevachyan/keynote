<template>
  <div class="notes-app">
    <div class="note-card" v-for="note in notes" :key="note.id">
      <h2 class="note-title">{{ note.title }}</h2>
      <p class="note-text">{{ note.text }}</p>
      <button class="note-button">Read More</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notes: [
        { id: 1, title: "Note 1", text: "This is the main text of note 1." },
        { id: 2, title: "Note 2", text: "This is the main text of note 2." },
        // Add more notes here
      ],
    };
  },
};
</script>
<style scoped>
body {
  background-color: #2e2e2e;
  color: #ffffff;
  font-family: Arial, sans-serif;
}

.notes-app {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  padding: 20px;
}

.note-card {
  background: #3c3c3c;
  border: 1px solid #555;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
  padding: 20px;
  width: 300px;
  display: flex;
  flex-direction: column;
  color: #ffffff;
}

.note-title {
  font-size: 1.5em;
  margin-bottom: 10px;
  color: #ffcc00;
  border-bottom: 1px solid #555;
  padding-bottom: 5px;
}

.note-text {
  font-size: 1em;
  margin-bottom: 20px;
  white-space: pre-wrap;
  color: #e0e0e0;
}

.note-button {
  align-self: flex-end;
  background: linear-gradient(135deg, #6a11cb, #2575fc);
  border: none;
  border-radius: 4px;
  color: #fff;
  cursor: pointer;
  font-size: 1em;
  padding: 10px 20px;
  transition: background 0.5s ease, transform 0.3s ease;
}

.note-button:hover {
  background: linear-gradient(135deg, #2575fc, #6a11cb);
  transform: scale(1.05);
}
</style>
