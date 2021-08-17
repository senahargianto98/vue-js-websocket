<template>
  <div id="app">
    <div v-for="message in messages" :key="message.text">
      <h1
      >{{ message.text }}</h1>
    </div>

    <form @submit.prevent="submit">
      <div class="field has-addons has-addons-fullwidth">
        <div class="control is-expanded">
          <input
            class="input"
            type="text"
            placeholder="Type a message"
            v-model="newMessage"
          />
        </div>
        <div class="control">
          <button
            type="submit"
            class="button is-danger"
            :disabled="!newMessage"
          >
            Send
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import axios from "axios";
export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      messages: [],
      newMessage: "",
    };
  },
  mounted() {
    Echo.channel("channel").listen("NewChatMessage", (e) => {
      this.messages.push({
        text: e.message,
      });
    });
  },
  methods: {
    submit() {
      axios
        .post(`http://localhost:8000/api/test`, {
          message: this.newMessage,
        })
        .then(
          (response) => {
            this.messages.push({
              text: this.newMessage,
            });

            this.newMessage = "";
          },
          (error) => {
            console.log(error);
          }
        );
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
