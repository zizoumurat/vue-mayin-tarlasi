<template>
  <div id="app">
    <Header />
    <login :user="user" v-if="!user.isLogin" />
    <game @newGame="newGame()" :user="user" :scores="scores" v-if="user.isLogin" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Login from "./components/Login.vue";
import Game from "./components/Game.vue";

export default {
  name: "app",
  data() {
    return {
      scores:[],
      user: {
        name: "",
        lastName: "",
        score: 0,
        isLogin: false,
        isFinished: false,
        selectedSize: 0
      }
    };
  },
  components: {
    Header,
    Login,
    Game
  },
  methods: {
    newGame(){
      this.user={
        name: "",
        lastName: "",
        score: 0,
        isLogin: false,
        isFinished: false,
        selectedSize: 0
      };
    }
  }
};
</script>