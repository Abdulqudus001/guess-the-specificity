<template>
  <div class="container">
    <h1>Guess The Specificity</h1>
    <new-selector @specificity="saveSpecificity" />
    <div class="specs">
      <div class="spec">
        <input type="text" readonly v-model="id">
        <div class="spec__control">
          <button @click="decrement($event, 'id')">-</button>
          <button @click="id++">+</button>
        </div>
      </div>
      <div class="spec">
        <input type="text" readonly v-model="className">
        <div class="spec__control">
          <button @click="decrement($event, 'className')">-</button>
          <button @click="className++">+</button>
        </div>
      </div>
      <div class="spec">
        <input type="text" readonly v-model="tag">
        <div class="spec__control">
          <button @click="decrement($event, 'tag')">-</button>
          <button @click="tag++">+</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    id: 0,
    className: 0,
    tag: 0,
    specificity: null,
  }),
  watch: {
    specificity(val) {
      console.log(val);
    },
  },
  methods: {
    decrement(e, key) {
      if (this[key] < 1) {
        e.preventDefault();
      } else {
        this[key] -= 1;
      }
    },
    saveSpecificity(e) {
      this.specificity = e;
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  color: #D62828;
  width: 700px;
  max-width: calc(100% - 40px);
  margin: 0 auto;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.specs {
  display: flex;
}

.spec {
  button {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    border: 1px solid transparent;
    font-size: 1.2rem;
    line-height: 0;
    background-color: #002B36;
    color: #fff;
    outline: none;
    cursor: pointer;
    &:focus {
      border: 1px solid rgba($color: #000000, $alpha: 0.5);
    }
  }
  input {
    font-size: 80px;
    text-align: center;
    margin: 10px;
    width: 150px;
    height: 150px;
    @media screen and (max-width: 600px) {
      width: 80px;
      height: 80px;
      font-size: 60px;
    }
  }
}
</style>
