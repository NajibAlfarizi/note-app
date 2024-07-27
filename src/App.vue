<script setup>
import { ref } from "vue";

const showForm = ref(false);

const newNote = ref("");

const notes = ref([]);

const errorMsg = ref("");

function addNotes(){
  if(!newNote.value){
    errorMsg.value = "Please enter a note";
    return;
  }
  notes.value.push({
    id: Date.now(),
    note: newNote.value,
    date: new Date().toLocaleDateString('en-GB'),
    backgroundColor: getRandomColor(),
  });

  newNote.value = "";
  showForm.value = false;
}

function getRandomColor(){
  return `#${Math.floor(Math.random()*16777215).toString(16)}`
}

</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="title">My Note</h1>
        <button @click="showForm = !showForm" class="add-btn" type="">+</button>
      </header>
      <div class="card-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{'background-color': note.backgroundColor}">
          <p class="card-content">
            {{ note.note }}
          </p>
          <p class="card-date">{{ note.date }}</p>
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
  height: 225px;
  background-color: #223358;
  border-radius: 10px;
  color: black;
  padding: 15px;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
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

.note {
  border-radius: 5px;
  border-color: #223358;
}

.form-error{
  font-style: italic;
  font-weight: light;
  color: red;
}
</style>
