<template>
  <div class="room">
    <h1>Room</h1>
    <button @click="copyURL">Copy URL</button>
    <button @mouseenter="qrModal = true" @mouseleave="qrModal = false">Qr Code<img v-if="qrModal" class="qr" :src="qr" /></button>
    <textarea v-model="text" min=0 @input="textUpdated" />
  </div>
</template>

<script>
import { io } from "socket.io-client";

const socket = io("http://localhost:3000");
import copy from "copy-to-clipboard";
import QRious from "qrious"
export default {
  data: () => ({
    text: "",
    qr: null,
    qrModal: false
  }),
  created() {
    const { room } = this.$route.query;
    if (room) {
      socket.emit("JOIN_ROOM", { room });
    } else {
      socket.emit("CREATE_ROOM");
    }

    socket.on("ROOM_CREATED", ({ room }) => {
      window.location = window.location + "?room=" + room;
    });

    socket.on("JOINED_TO_ROOM", async () => {
      socket.on("TEXT_UPDATE", ({ text }) => {
        this.text = text;
        
        navigator.clipboard.writeText(this.text).catch(() => {
            console.log("windows is not focused")
        })
          
        
        
      });
    });

    window.onfocus = () => {
      navigator.clipboard.writeText(this.text)
    }

     this.qr = new QRious({
          size: 300,
          value: window.location.toString(),
     }).toDataURL();
  },
  methods: {
    textUpdated(event) {
      socket.emit("TEXT_UPDATE", { text: event.target.value });
    },
    copyURL(){
      copy(window.location)
    }
  },
};
</script>

<style scoped>
  textarea {
    display: block;
    margin: auto;
    margin-top: 15px;
    height: 50vh;
    width: 60vw;
    border: none;
    background: rgb(218, 218, 218);
    border-radius: 30px;
    padding: 40px;
    font-size: 18px;
  }

  textarea:focus{
    outline: none;
    background: rgb(236, 236, 236);
  }

  .qr {
    position: absolute;
    border-radius: 10px;
    border: 5px solid black;
  }
</style>