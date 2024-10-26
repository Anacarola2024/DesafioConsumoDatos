<script >

import axios from 'axios';

export default {
  data() {
    return {
      user1: null,
      user2: null,
      message1: '',
      message2: '',
      messages: []
    };
  },

   async mounted() {
    axios.get(`https://randomuser.me/api/?results=2&t=${Math.random()}`)
      .then(response => {
        this.user1 = response.data.results[0];
        return axios.get(`https://randomuser.me/api/?results=2&t=${Math.random()}`);
      })
      .then(response => {
        this.user2 = response.data.results[0];
      })
      .catch(error => {
        console.error(error);
      });
  },

  methods: {
    sendMessage(user) {
      const message = user === 'user1' ? this.message1 : this.message2;
      const userName = user === 'user1' ? this.user1.name.first : this.user2.name.first;

      if (message.trim() !== '') {
        this.messages.push({
          id: Date.now(),
          user: userName,  
          text: message
        });
        
        
        if (user === 'user1') {
          this.message1 = '';
        } else {
          this.message2 = '';
        }
      
        };
      }
    }

  }




</script>

<template>

<div id="app">
    <div class="chat-container">
      <div class="user-section">
        <img v-if="user1?.picture?.large" :src="user1.picture.large" alt="Foto del usuario 1">
        <h3>{{ user1?.name?.first }} {{ user1?.name?.last }}</h3>
        <input v-model="message1" type="text" placeholder ="Escribe un mensaje..." />
        <button @click="sendMessage('user1')">Enviar</button>
      </div>
      <div class="chat-box">
        
        <div class="messages">
        <div v-for="msg in messages" :key="msg.id" :class="['message', msg.user === user1.name.first ? 'user1' : 'user2']">
          <strong class="user-name">{{ msg.user }}:</strong>
          <p>{{ msg.text }}</p>

        </div>
        </div>
        
      </div>

      <div class="user-section">
        <img v-if="user2?.picture?.large" :src="user2.picture.large" alt="Foto del usuario 2">
        <h3>{{ user2?.name?.first }} {{ user2?.name?.last }}</h3>
        <input v-model="message2" type="text" placeholder="Escribe un mensaje..." />
        <button @click="sendMessage('user2')">Enviar</button>
      </div>
    </div>
  </div>





 
</template>

<style >

.chat-container {
    display: flex;
    width: 800px;
    border: 1px solid #ccc;
    border-radius: 10px;
    overflow : hidden;
    background-color: #fff;
}

.user-section {
    width: 200px;
    background-color: #fafafa;
    border-right: 1px solid #ccc;
    text-align: center;
    padding: 10px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.chat-box {
    flex-grow: 1;
    padding: 10px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    background-color: #f9f4ed;
    position: relative;
    overflow-y: auto;
    height: 400px;
}

.messages {
  overflow-y: auto;
  overflow-x: hidden;
  max-height: 340px;
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}



.message {
    padding: 8px;
    margin: 5px 0;
    border-radius: 5px;
    color: #fff;
    background-color: #4CAF50; 
    align-self: flex-start; 
}

.user2 {
    background-color: #2196F3; 
    align-self: flex-end; 
}

</style>
