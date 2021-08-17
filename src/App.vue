<template>
  <div id="app">
    <div v-for="txt in messages" :key="txt.id">
      <div v-for="txt1 in txt" :key="txt1.id">
        <div v-for="txt2 in txt1" :key="txt2.id">
          <div v-for="txt3 in txt2" :key="txt3.id">
            {{ txt3.message }}
          </div>
        </div>
      </div>
    </div>

    <form @submit.prevent="submit">
      <div class="field has-addons has-addons-fullwidth">
        <div class="control is-expanded">
          <input
            class="input"
            type="text"
            placeholder="Type a message"
            v-model="message"
          />
        </div>
        <div class="control">
          <button type="submit" class="button is-danger">Send</button>
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
      message: "",
    };
  },
  mounted() {
    window.Echo.channel("channel").listen("Hello", (e) => {
      this.messages.push({
        text: e,
      });
    });
  },
  methods: {
    submit() {
      axios
        .post(`http://localhost:8000/api/test`, {
          message: this.message,
        })
        .then(
          axios
          .get('http://localhost:8000/api/testing')
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
