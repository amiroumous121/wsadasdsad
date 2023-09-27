<script>
export default {
  name: "Chatbot",
  data() {
    return {
      userMessage: "",
      chatHistory: [],
    };
  },
  methods: {
    sendMessage() {
      this.chatHistory.push({ type: "user", message: this.userMessage });
      const botResponse = this.generateBotResponse(
        this.userMessage.toLowerCase()
      );
      this.chatHistory.push({ type: "bot", message: botResponse });
      this.userMessage = "";
    },
    generateBotResponse(userMessage) {
      if (userMessage.includes("hello")) {
        return "Hi there! How can I assist you today?";
      }
      if (userMessage.includes("recipe")) {
        return "Would you like to search for a specific recipe or browse categories?";
      }
      if (userMessage.includes("search")) {
        return "What recipe are you looking for?";
      }
      if (userMessage.includes("categories")) {
        return "We have various categories like Desserts, Main Course, and Snacks. Which one interests you?";
      }
      // ... add more conditions here
      return "I'm sorry, I didn't understand that. Could you please rephrase?";
    },
  },
};
</script>

<template>
  <div class="chatbot-container">
    <div class="chat-window">
      <div v-for="(chat, index) in chatHistory" :key="index">
        <div :class="chat.type">{{ chat.message }}</div>
      </div>
    </div>
    <div class="chat-input">
      <input
        type="text"
        v-model="userMessage"
        @keyup.enter="sendMessage"
        placeholder="Type your message..."
      />
    </div>
  </div>
</template>

<style>
.chatbot-container {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 300px;
  height: 400px;
  border: 1px solid #ccc;
  background-color: #fff;
  z-index: 1000;
}
.chat-window {
  height: 360px;
  overflow-y: auto;
}
.chat-input {
  height: 40px;
}
.user {
  color: blue;
}
.bot {
  color: green;
}
</style>
