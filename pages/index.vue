<template>
  <div class="container">
    <h1>Guess The Specificity</h1>
    <new-selector @specificity="saveSpecificity" @new="reset" />
    <h3 v-if="correct" class="correct">
      Awesome, you rock 👍
    </h3>
    <h3 v-if="wrong">
      oops😔...Try again
    </h3>
    <div class="specs">
      <div class="spec">
        <h2>ID</h2>
        <input type="text" readonly v-model="id">
        <div class="spec__control">
          <button @click="decrement($event, 'id')">-</button>
          <button @click="id++">+</button>
        </div>
      </div>
      <div class="spec">
        <h2>Classes & Pseudo classes</h2>
        <input type="text" readonly v-model="className">
        <div class="spec__control">
          <button @click="decrement($event, 'className')">-</button>
          <button @click="className++">+</button>
        </div>
      </div>
      <div class="spec">
        <h2>Tag Names</h2>
        <input type="text" readonly v-model="tag">
        <div class="spec__control">
          <button @click="decrement($event, 'tag')">-</button>
          <button @click="tag++">+</button>
        </div>
      </div>
    </div>
    <button v-show="hideButton === false" @click="showResults">View Results</button>
  </div>
</template>

<script>
export default {
  data: () => ({
    id: 0,
    className: 0,
    tag: 0,
    specificity: null,
    correct: null,
    wrong: null,
    wrongCount: 0,
    hideButton: false,
  }),
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
    showResults() {
      this.wrong = false;
      this.correct = false;
      const userResult = [0, this.id, this.className, this.tag].join();
      const { specificity } = this.specificity[0];
      if (userResult === specificity) {
        this.hideButton = true;
        this.correct = true;
        this.wrongCount = 0;
      } else {
        this.wrong = true;
        this.wrongCount += 1;
      }
    },
    reset() {
      this.id = 0;
      this.className = 0;
      this.tag = 0;
      this.correct = null;
      this.wrong = null;
      this.wrongCount = 0;
      this.hideButton = false;
    },
  },
};
</script>

<style lang="scss" scoped>
h1 {
  margin: 10px 0;
}

.container {
  color: #45969b;
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
  margin: 20px 0;
}

.spec {
  h2 {
    height: 40px;
    font-size: 1rem;
    max-width: 150px;
    line-height: 1;
    color: #002B36;
  }

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
    color: #002B36;
    @media screen and (max-width: 600px) {
      width: 80px;
      height: 80px;
      font-size: 60px;
    }
  }
}

.specs + button {
  border: 1px solid transparent;
  cursor: pointer;
  color: #fff;
  background-color: #45969b;
  padding: 10px 20px;
  border-radius: 4px;
  outline: none;
  &:focus {
    border: 1px solid rgba(0, 0, 0, 0.5);
  }
}

.correct {
  color: green;
}
</style>
