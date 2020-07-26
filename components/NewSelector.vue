<template>
  <div class="selector">

  </div>
</template>

<script>
import classes from '~/db/classes';
import ids from '~/db/ids';

export default {
  mounted() {
    const selectorObj = this.generateSelectorObj(3);
    const selectorString = this.createSelectorString(selector);
    console.log(selectorString);
  },
  methods: {
    generateNumber(max) {
      return Math.floor(Math.random() * max);
    },
    getSelectorType() {
      return this.getRandomElement(['class', 'id']);
    },
    includePseduo() {
      return Math.floor(Math.random() * 10) === 4;
    },
    getSelectorValue(selectorType) {
      if (selectorType === 'class') {
        return this.getRandomElement(classes);
      }
      if (selectorType === 'id') {
        return this.getRandomElement(ids);
      }
      return this.getRandomElement(classes);
    },
    getRandomElement(arr) {
      const randomNum = Math.floor(Math.random() * arr.length);
      return arr[randomNum];
    },
    generateSelectorObj(num) {
      const selectorType = this.getSelectorType();
      const selectorValue = this.getSelectorValue(selectorType);
      const pseudoElement = this.includePseduo() && this.getRandomElement(pseudoElements);
      const pseudoClass = this.includePseduo() && this.getRandomElement(pseudoClasses);
      const selector = {
        selectorType,
        selectorValue,
        pseudoElement,
        pseudoClass,
        children: [],
      };

      if (num > 0) {
        while (num > 0) {
          const newChildren = this.generateNumber(num);
          selector.children.push(this.generateSelector(newChildren));
          num -= 1;
        }
      }

      return selector;
    },
    createSelectorString(selectorObj) {
      let html = '';
      if (selectorObj.pseudoEl || selectorObj.pseudoClass) {
        html = selectorObj.selectorValue + selectorObj.pseudoElement + selectorObj.pseudoClass;
      } else {
        console.log(selectorObj.selectorValue);
        html = selectorObj.selectorValue;
      }
      if (selectorObj.children.length) {
        return `${html} ${this.createSelectorString(selectorObj.children[0])}`;
      }

      return html;
    },
  },
};
</script>

<style lang="scss" scoped>
.selector {
  width: 100%;
  padding: 10px;
  max-width: calc(100% - 20px);
  margin: 0 auto;
  background-color: #002B36;
}
</style>
