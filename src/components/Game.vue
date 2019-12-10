<template>
  <div class="container">
    <div class="row">
      <div class="score-row">
        <div class="score-element">
          <span>Name</span>
          <span>{{user.name}}</span>
        </div>
        <div class="score-element">
          <span>Score</span>
          <span>{{user.score}}</span>
        </div>
        <button class="btn btn-restart" @click="restartGame()">Restart</button>
      </div>
    </div>
    <div class="row">
      <div class="game-content">
        <div class="game-boxes">
          <template v-for="(item,ind) in arrBox">
            <box
              @clickedBomb="clickedBomb()"
              @gameFinished="gameFinished($event)"
              :key="'box-'+ind"
              :user="user"
              :box="item"
              :finished="gameIsFinished"
            >{{item}}</box>
            <br v-if="(ind+1)%user.selectedSize==0 && ind>0" :key="'br'+ind" />
          </template>
        </div>
        <top-score :scores="scores" />
      </div>
    </div>
    <div
      class="modal fade"
      v-bind:class="{show:showModal}"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
      :style="[showModal?{'display':'block'}:{'display':'none'}]"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">GAME OVER</div>
          <div class="modal-body">
            <p>
              <span class="float-left">Your Score</span>
              <span class="float-right">
                <strong>{{this.user.score}}</strong>
              </span>
            </p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-try-again" @click="restartGame()">Try Again</button>
            <button type="button" class="btn btn-new-game" @click="newGame()">New Game</button>
          </div>
        </div>
      </div>
    </div>
    <div class="modal-backdrop fade show" v-if="showModal"></div>
  </div>
</template>

<script>
import Box from "./Box.vue";
import TopScore from "./TopScores.vue";

export default {
  data() {
    return {
      boxCount: this.user.selectedSize * this.user.selectedSize,
      arrBox: [],
      gameIsFinished: false,
      showModal: false
    };
  },
  methods: {
    randomNumber() {
      return Math.floor(
        Math.random() * (this.user.selectedSize * this.user.selectedSize)
      );
    },
    clickedBomb() {
      for (var i = 0; i < this.arrBox.length; i++) {
        if (this.arrBox[i].bomb) this.arrBox[i].class = "red";
      }
    },
    restartGame() {
      this.showModal = false;
      this.user.score = 0;
      this.gameIsFinished = false;
      this.initialBombs();
    },
    newGame() {
      this.showModal = false;
      this.gameIsFinished = false;
      this.$emit("newGame");
    },
    initialBombs() {
      this.arrBox = [];
      var length = 0;
      for (var i = 0; i < this.boxCount; i++) {
        this.arrBox.push({ bomb: false, class: "default", clicked: false });
      }
      var bombss = [];
      while (length < this.user.selectedSize) {
        var rnd = this.randomNumber();
        if (this.arrBox[rnd].bomb == false) {
          bombss.push(rnd);
          this.arrBox[rnd].bomb = true;
          length++;
        }
      }
      console.log("bombalar! ", bombss);
    },
    gameFinished(user) {
      this.gameIsFinished = true;
      this.scores.push({
        name: user.name,
        lastName: user.lastName,
        score: user.score
      });
      this.showModal = true;
      setTimeout(function() {
        this.showModal = true;
      }, 4000);
    }
  },
  props: ["user", "scores"],
  components: {
    Box,
    TopScore
  },
  created() {
    this.initialBombs();
  }
};
</script>

<style scoped>
.score-element {
  display: inline-block;
  margin: 0 5px;
  width: 390px;
  line-height: 60px;
  background-color: #f7f9fa;
  padding: 0 25px;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  border-radius: 10px;
  vertical-align: middle;
}
.score-element span:first-child {
  float: left;
  color: #91949f;
}
.score-element span:last-child {
  float: right;
  color: #5d6980;
  font-weight: 500;
}
.btn-restart {
  background-color: #ff8a00;
  width: 158px;
  height: 60px;
  border: none;
  outline: none;
  color: #fff;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  border-radius: 10px;
}
.game-content {
  margin-top: 30px;
  width: 100%;
}

.modal-dialog {
  transition: all 1s ease-in-out !important;
  width: 430px;
  border-radius: 15px;
}
.modal.show .modal-dialog {
  top: 100px;
}
.modal-header {
  background-color: #00bce5;
  display: inline-block;
  text-align: center;
  color: #fff;
  font-weight: bold;
}
.modal-footer {
  justify-content: center;
}
.btn-try-again {
  background-color: #ff8a00;
  color: #fff;
}
.btn-new-game {
  background-color: #25a35c;
  color: #fff;
}
@media (min-width: 1200px) {
  .game-boxes {
    display: inline-block;
    width: 520px;
    vertical-align: top;
  }
}
</style>