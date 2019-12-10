<template>
  <ul class="list-group">
    <li class="list-group-item active">Top 10 Scores</li>
    <li v-for="(score,index) in sortedScores" class="list-group-item" v-bind:key="index">
      <span>{{(index+1)}} .Score</span>
      <span>{{score.name}} {{score.lastName}} ({{score.score}})</span>
    </li>
  </ul>
</template>
<script>
export default {
  data() {
    return {};
  },
  props: ["scores"],
  methods: {},
  computed: {
    sortedScores() {
      var list= this.scores.sort(function(a, b) {
        return b.score - a.score;
      });
      if(list.length>10)
       list=list.slice(0,10);
    return list;
    } 
  }
};
</script>
<style scoped>
.list-group {
  width: 430px;
  display: inline-block;
  margin: 10px 20px;
  background-color: #f7f9fa;
  min-height: 300px;
}
.list-group li {
  border: none;
  display: inline-block;
  width: 100%;
  background: none;
}
.list-group li.active {
  background-color: #00bce5;
  line-height: 40px;
}
.list-group span:first-child {
  float: left;
  color: #91949f;
}
.list-group span:last-child {
  float: right;
  color: #5d6980;
}
</style>