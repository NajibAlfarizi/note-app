<template>
  <main>
    <div class="container">
      <header>
        <h1 class="title">My Note</h1>
        <button class="add-btn" type="">+</button>
      </header>
      <div class="card-container">
        <div class="card">
          <p class="card-content">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Ducimus,
            numquam asperiores repellat, cupiditate animi fugit similique
          </p>
          <p class="card-date">27-07-2024</p>
        </div>
        <div class="card">
          <p class="card-content">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Ducimus,
            numquam asperiores repellat, cupiditate animi fugit similique
          </p>
          <p class="card-date">27-07-2024</p>
        </div>
        <div class="card">
          <p class="card-content">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Ducimus,
            numquam asperiores repellat, cupiditate animi fugit similique
          </p>
          <p class="card-date">27-07-2024</p>
        </div>
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
  color: white;
  padding: 15px;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>
