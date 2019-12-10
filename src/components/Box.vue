<template>
  <div
    class="box"
    :class="[box.class]"
    :style="{'width':520/user.selectedSize-10+'px','height':520/user.selectedSize-10+'px'}"
    @click="clickedBox()"
  ></div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: ["box", "user", "finished"],
  methods: {
    clickedBox() {
      if (!this.finished && !this.box.isClicked) {
        if (this.box.bomb) {
          this.box.class = "red";
          this.$emit("clickedBomb");
          this.$emit("gameFinished", this.user);
        } else {
          this.user.score += 5;
          this.box.class = "green";
          this.box.isClicked = true;
        }
      }
    }
  }
};
</script>

<style>
.box {
  display: inline-block;
  margin: 5px;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  border-radius: 10px;
}


.default {
  background-color: #5d6980;
}
.red {
  background: #fa3a3a url("../assets/images/bomb.svg") no-repeat center center;
  background-size:60%;
}
.green {
  background: #25a35c url("../assets/images/ok.svg") no-repeat center center;
  background-size:60%;

}
</style>