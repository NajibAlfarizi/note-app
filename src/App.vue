<script setup>
import { ref, onMounted, watch } from 'vue';

const showForm = ref(false);
const newNote = ref("");
const newTitle = ref("");
const notes = ref([]);
const errorMsg = ref("");
const selectedNote = ref(null);

onMounted(() => {
  const savedNotes = localStorage.getItem('notes');
  if (savedNotes) {
    notes.value = JSON.parse(savedNotes);
  }
});

watch(notes, (newNotes) => {
  localStorage.setItem('notes', JSON.stringify(newNotes));
}, { deep: true });

function addNotes() {
  if (!newNote.value || !newTitle.value) {
    errorMsg.value = "Please enter both title and note";
    return;
  }
  const backgroundColor = getRandomColor();
  const textColor = getContrastColor(backgroundColor);

  notes.value.push({
    id: Date.now(),
    title: newTitle.value,
    note: newNote.value,
    date: new Date().toLocaleDateString('en-GB'),
    backgroundColor,
    textColor,
  });

  newNote.value = "";
  newTitle.value = "";
  showForm.value = false;
}

function getRandomColor() {
  let color;
  do {
    color = `#${Math.floor(Math.random() * 16777215).toString(16)}`;
  } while (isTooLight(color));
  return color;
}

function isTooLight(hex) {
  const r = parseInt(hex.slice(1, 3), 16);
  const g = parseInt(hex.slice(3, 5), 16);
  const b = parseInt(hex.slice(5, 7), 16);
  
  const luminance = 0.299 * r + 0.587 * g + 0.114 * b;
  return luminance > 200;
}

function getContrastColor(hex) {
  const r = parseInt(hex.slice(1, 3), 16);
  const g = parseInt(hex.slice(3, 5), 16);
  const b = parseInt(hex.slice(5, 7), 16);
  
  const luminance = 0.299 * r + 0.587 * g + 0.114 * b;
  return luminance > 186 ? "#000000" : "#FFFFFF";
}

function deleteNote(id) {
  notes.value = notes.value.filter(note => note.id !== id);
}

function showNoteDetail(note) {
  selectedNote.value = note;
}

function closeNoteDetail() {
  selectedNote.value = null;
}

</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="title">My Note</h1>
        <button @click="showForm = !showForm" class="add-btn">+</button>
      </header>
      <div class="card-container">
        <div 
          v-for="note in notes" 
          :key="note.id" 
          class="card" 
          :style="{ 'background-color': note.backgroundColor, color: note.textColor }" 
          @click="showNoteDetail(note)"
        >
          <button @click.stop="deleteNote(note.id)" class="delete-btn">-</button>
          <p class="card-content">{{ note.title }}</p>
          <p class="card-date" :style="{ color: note.textColor }">{{ note.date }}</p>
        </div>
      </div>
    </div>
    <div v-if="showForm" class="form-overlay">
      <div class="form-modal">
        <h2 class="form-title">What happens today??</h2>
        <button @click="showForm = !showForm" class="form-close-btn">
          &times;
        </button>
        <p v-if="errorMsg" class="form-error">{{ errorMsg }}</p>
        <input
          v-model="newTitle"
          class="title-input"
          type="text"
          placeholder="Enter title here..."
        />
        <textarea
          v-model="newNote"
          class="note"
          name="note"
          id="note"
          cols="30"
          rows="10"
          placeholder="Enter your note here..."
        ></textarea>
        <button @click="addNotes" class="form-save-btn">Save</button>
      </div>
    </div>
    <div v-if="selectedNote" class="overlay" @click="closeNoteDetail">
      <div class="note-detail" @click.stop>
        <h2>{{ selectedNote.title }}</h2>
        <p>{{ selectedNote.note }}</p>
        <p class="card-date">{{ selectedNote.date }}</p>
        <button @click="closeNoteDetail" class="close-detail-btn">&times;</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 10px;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.title {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: #223358;
}

.add-btn {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  font-weight: bold;
  font-size: 24px;
  border-radius: 100%;
  color: white;
  background-color: #223358;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  width: 225px;
  height: 150px;
  border-radius: 10px;
  padding: 15px;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
}

.card-content{
  font-weight: bolder;
  font-size: 30px;
}

.delete-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 30px;
  height: 30px;
  background: rgb(253, 109, 109);
  border: none;
  border-radius: 100%;
  cursor: pointer;
  color: white;
  font-weight: bold;
  font-size: 24px;
}

.card-date {
  position: absolute;
  bottom: 10px;
  right: 10px;
  margin: 0;
  font-size: 14px;
  color: #000;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
}

.form-modal {
  width: 420px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 15px;
  align-self: center;
}

.form-save-btn {
  padding: 5px 10px;
  font-size: 20px;
  width: 30%;
  background-color: #223358;
  border: none;
  cursor: pointer;
  border-radius: 10px;
  margin-top: 15px;
  color: white;
  display: flex;
  justify-content: center;
  align-self: center;
}

.form-close-btn {
  position: absolute;
  top: 5px;
  right: 10px;
  width: 30px;
  height: 30px;
  cursor: pointer;
  border: none;
  border-radius: 100%;
  background-color: #223358;
  color: white;
  font-size: 24px;
}

.title-input {
  border-radius: 5px;
  border: 1px solid #223358;
  margin-bottom: 10px;
  padding: 5px;
}

.note {
  border-radius: 5px;
  border: 1px solid #223358;
}

.form-error {
  font-style: italic;
  font-weight: light;
  color: red;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 20;
  display: flex;
  justify-content: center;
  align-items: center;
}

.note-detail {
  width: 420px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.close-detail-btn {
  position: absolute;
  top: 5px;
  right: 10px;
  width: 30px;
  height: 30px;
  cursor: pointer;
  border: none;
  border-radius: 100%;
  background-color: #223358;
  color: white;
  font-size: 24px;
}
</style>
