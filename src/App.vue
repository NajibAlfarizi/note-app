<template>
  <main>
    <div class="container">
      <header>
        <h1 class="title">My Note</h1>
        <button class="add-btn" type="">+</button>
      </header>
      <div>[card]</div>
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
  background-color: #223358;;
}
</style>
