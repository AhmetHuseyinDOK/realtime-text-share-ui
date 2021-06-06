<template>
  <div class="home">
    <h1>RealTime Text Editing &<br> Sharing Tool</h1>
    <button @click="createRoom">CREATE A ROOM</button>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  components: {},
  methods: {
    createRoom() {
      window.location = window.location + "room";
    },
  },
};
</script>

<style>
body {
  margin: 0px !important;
  padding: 0px !important;
}

.home {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

  button {
    padding: 15px;
    border-radius: 15px;
    border: none;  
    background-color: rgba(39, 46, 146, 0.815);
    color: white;
    font-weight: bold;
    margin-right: 10px;
  }

  button:hover {
    background-color: rgba(39, 46, 146, 0.5);
  }
</style>