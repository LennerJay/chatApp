<template>
  <div class="container">
    <div class="header">
      <div class="logo1">
        <img src="../assets/litchat.ico" alt="logo" />
      </div>
      <div class="header-profile">
        <span>Welcome Mr.asdasd</span>
        <img src="../assets/profile.png" alt="" />
      </div>
    </div>
    <div class="content-wrapper">
      <div class="message-box">
        <div class="chat-container">
          <div class="chats">
            <div class="chat-wrapper">
              <h1>CHATS</h1>
              <input id="search" type="text" placeholder="Search" />
              <div class="recent-chat">
                <div class="profile-1">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
                <div class="profile-1">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
                <div class="profile-1">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
                <div class="profile-1">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
              </div>
              <h2>Friend Suggestion</h2>
              <div class="friend-suggestion">
                <div class="profile-2">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
                <div class="profile-2">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
                <div class="profile-2">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
                <div class="profile-2">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
                <div class="profile-2">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
                <div class="profile-2">
                  <img src="../assets/profile.png" alt="" />
                  <span>Manilyn</span>
                </div>
              </div>
            </div>
          </div>
          <div class="chat-box">
            <div class="box-wrapper">
              <div class="chat-profile">
                <img src="../assets/profile.png" alt="" srcset="" />
                <span>Jeanee</span>
              </div>
              <div class="option">
                <img src="../assets/exclamation-mark.png" alt="" srcset="" />
              </div>
            </div>
            <div class="message-wrapper">
              <div class="message-content">
                <ul>
                  <li v-for="message in socketStore.messages">
                    {{ message.username }} : {{ message.message }}
                  </li>
                </ul>
              </div>
              <div class="send-message">
                <div class="send-message-wrapper">
                  <input v-model="messageInput" id="message-input" type="text" />
                  <img
                    class="send"
                    id="send-icon"
                    src="../assets/send-icon.png"
                    alt=""
                    @click="sendMessage"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="profile-overview">
        <div class="user-profile">
          <img src="../assets/profile.png" alt="" />
          <span>{{ userInfo.user_name }}</span>
        </div>
        <div class="personalities">
          <div class="personalities-container">
            <span>Email: {{ userInfo.email }}</span>
            <span>Gender: {{ userInfo.gender }}</span>
            <span>Birthday: {{ userInfo.birthday }}</span>
          </div>
        </div>

        <div class="buttons">
          <button class="add">Add Chat</button>
          <button class="block">Blocked</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { io } from "socket.io-client";
import { ref, onMounted } from "vue";
import { useSocketStore } from "../stores/socket";

const socketStore = useSocketStore();

const userInfo = ref([]);
const token = ref("");
const messageInput = ref("");

const messages = ref([]);

const sendMessage = () => {

  socketStore.sendMessage(messageInput.value,userInfo.value.user_name,userInfo.value.id);
  messageInput.value = "";

};

onMounted(() => {
  if (localStorage.getItem("user")) {
    userInfo.value = JSON.parse(localStorage.getItem("user"));
  }
  if (localStorage.getItem("token")) {
    token.value = JSON.parse(localStorage.getItem("token"));
  }
  socketStore.connect();
  socketStore.receiveMessage();
});
</script>

<style scoped>
.container {
  background: #9452ff;
  padding: 20px;
  border-radius: 20px;
}
h1 {
  color: black;
}

.header {
  display: flex;
  justify-content: space-between;
}
.logo1 img {
  width: 130px;
}
.header-profile {
  display: flex;
  align-items: center;
}
.header-profile span {
  color: white;
  font-size: larger;
}
.header-profile img {
  width: 50px;
}
.content-wrapper {
  width: 99%;
  border-radius: 20px;
  display: flex;
  background: white;
}
.message-box {
  width: 70%;
  border-radius: 20px;
}
.profile-overview {
  width: 29%;
  border-radius: 20px;
  display: grid;
  grid-template-rows: 1fr 1fr 1fr 0.3fr;
  background: #9452ff;
  height: 615px;
  padding: 2px;
}
h2 {
  color: black;
}
.chat-container {
  display: flex;
  margin: 10px;
}
.chats {
  width: 30%;
  border: 2px solid white;
  margin-right: 20px;
  background: white;
  border-radius: 15px;
}
.chat-box {
  width: 70%;
  border: 2px solid white;
  display: grid;
  grid-template-rows: 0.1fr 1fr;
  background: #9452ff;
  border-radius: 20px;
}
.chat-wrapper {
  width: auto;
  display: flex;
  flex-direction: column;
  background: #9452ff;
  border-radius: 20px;
}
.chat-wrapper input {
  width: 100%;
}
#search {
  width: auto;
  margin: 10px;
}
.recent-chat {
  justify-content: center;
}
.recent-chat,
.friend-suggestion {
  display: grid;
  grid-template-columns: auto;
  height: 20vh;
  overflow-y: auto;
}
.friend-suggestion {
  grid-template-columns: auto auto;
  margin: 0 10px;
}
.profile-1 {
  display: flex;
  justify-content: center;
  margin-bottom: 12px;
  color: black;
}
.profile-1 img,
.profile-2 img,
.chat-profile img {
  width: 50px;
  height: 50px;
}
.profile-2 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 12px;
  color: black;
}
.box-wrapper {
  margin: 10px;
  display: flex;
  justify-content: space-between;
  padding: 5px;
  background: #9452ff;
  border-radius: 7px;
}
.chat-profile {
  color: black;
}
.option {
  display: flex;
  align-items: center;
}
/* .option{

} */
.message-wrapper {
  margin: 10px;
  border: 2px solid white;
  display: grid;
  grid-template-rows: 1fr 0.2fr;
  background: white;
  border-radius: 20px;
}
.message-content,
.send-message {
  margin: 5px;
  border: 2px solid white;
}
.message-content {
  height: 100%;
  overflow-y: auto;
}
.user1,
.user2 {
  display: flex;
  align-self: flex-end;
}
.send-message-wrapper {
  display: flex;
  justify-content: center;
  margin: 0 10px;
  background: white;
  border-radius: 10px;
}
#message-input {
  width: 700px;
  height: 30px;
  border-radius: 30px;
  background: white;
}
#send-icon {
  width: 50px;
  height: 50px;
}
.user-profile,
.personalities,
.bio,
.buttons {
  display: flex;
  justify-content: center;
  border-radius: 20px;
}
.user-profile {
  flex-direction: column;
  align-items: center;
  border-radius: 20px;
}
.user-profile img {
  width: 150px;
  height: 150px;
}

.personalities-container {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  border: 2px solid white;
  margin: 10px;
  overflow: auto;
}
.personalities span {
  margin-left: 10px;
}
.add {
  background-color: #0000ff99;
  border-radius: 5px;
  height: 30px;
  padding: 5px;

  margin-right: 5px;
}
.add:hover {
  background-color: violet;
  color: blue;
}
.block {
  background-color: #7c0505;
  border-radius: 5px;
  height: 30px;
  padding: 5px;
  margin-right: 6px;
}
.block:hover {
  background-color: violet;
  color: #7c0505;
}
</style>
