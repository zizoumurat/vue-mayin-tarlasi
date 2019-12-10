<template>
  <div class="container form-container">
    <div class="row">
      <div class="col-lg-12 no-padding">
        <form>
          <div class="form-group">
            <label>Name</label>
            <input type="text" class="form-control" v-model="user.name" />
          </div>
          <div class="form-group">
            <label>Surname</label>
            <input type="text" class="form-control" v-model="user.lastName" />
          </div>
          <div class="form-group">
            <label>Choose Size</label>
            <select class="form-control" v-model="user.selectedSize">
              <option v-for="size in sizes" :value="size" v-bind:key="size">{{size}}x{{size}} Grid</option>
            </select>
          </div>
          <button type="button" class="btn btn-success" @click="loginUser">Start Game</button>
        </form>
        <div class="alert alert-danger" role="alert" v-if="err.length>0">{{err}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sizes: [5, 6, 7, 8],
      err: ""
    };
  },
  props: ["user"],
  methods: {
    loginUser() {
      if (
        this.user.name &&
        this.user.name.length > 0 &&
        this.user.selectedSize &&
        this.user.selectedSize > 0
      ) {
        this.err = "";
        this.user.isLogin = true;
      } else {
        this.err = "Oyuna başlamak için bilgilerinizi girin";
      }
    }
  }
};
</script>

<style scoped>
@media (min-width: 1200px) {
  .form-container {
    max-width: 520px;
  }
}

.btn {
  width: 100%;
  background-color: #25a35c;
  border: none;
}
label {
  color: #91949f;
}
.form-control {
  background-color: #f7f9fa;
  color: #5d6980;
  outline: none;
  border: none;
}
.form-control:focus {
  background-color: #f7f9fa;
}
.alert {
  margin-top: 1rem;
}
</style>