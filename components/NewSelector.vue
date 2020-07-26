<template>
  <div class="selector">
    <div class="selectors"></div>
    <button @click="generateNewSelector">New Selector</button>
  </div>
</template>

<script>
import classes from '~/db/classes';
import ids from '~/db/ids';
import tags from '~/db/tagNames';
import pseudoClasses from '~/db/pseudoClasses';

export default {
  mounted() {
    this.generateNewSelector();
  },
  methods: {
    generateNewSelector() {
      const selectorObj = this.generateSelectorObj(3);
      const selectors = this.createSelector(selectorObj);
      this.addSelectorToPage(selectors);
      console.log(selectors);
    },
    addSelectorToPage(selectors) {
      const selectorField = document.querySelector('.selectors');
      selectorField.textContent = '';
      const fragment = document.createDocumentFragment();
      const seperators = [' ', '', ' > ', ' ~ ', ' + '];
      selectors.forEach((selector, index) => {
        const selectorType = document.createElement('span');
        const seperator = document.createElement('span');
        selectorType.textContent = selector.value;
        selectorType.classList.add(selector.type);
        if (index < selectors.length - 1) {
          seperator.textContent = this.getRandomElement(seperators);
        }
        fragment.append(selectorType, seperator);
      });
      selectorField.append(fragment);
    },
    generateNumber(max) {
      return Math.floor(Math.random() * max);
    },
    getSelectorType() {
      return this.getRandomElement(['class', 'id', 'tag']);
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
      if (selectorType === 'tag') {
        return this.getRandomElement(tags);
      }
      return this.getRandomElement(tags);
    },
    getRandomElement(arr) {
      const randomNum = Math.floor(Math.random() * arr.length);
      return arr[randomNum];
    },
    generateSelectorObj(num) {
      const selectorType = this.getSelectorType();
      const selectorValue = this.getSelectorValue(selectorType);
      // const pseudoElement = this.includePseduo() && this.getRandomElement(pseudoElements);
      const pseudoClass = this.includePseduo() && this.getRandomElement(pseudoClasses);
      const selector = {
        selectorType,
        selectorValue,
        // pseudoElement,
        pseudoClass,
        children: [],
      };

      if (num > 0) {
        while (num > 0) {
          const newChildren = this.generateNumber(num);
          selector.children.push(this.generateSelectorObj(newChildren));
          num -= 1;
        }
      }

      return selector;
    },
    createSelector(selectorObj) {
      const selector = [];
      let current = selectorObj;
      while (current.children.length > 0) {
        selector.push({
          type: current.selectorType,
          value: current.selectorValue,
        });
        if (current.pseudoClass) {
          selector.push({
            type: 'pseudo',
            value: current.pseudoClass,
          });
        }
        [current] = current.children;
      }
      selector.push({
        type: current.selectorType,
        value: current.selectorValue,
      });

      return selector;
    },
  },
};
</script>

<style lang="scss" scoped>
.selector {
  width: 100%;
  padding: 10px;
  max-width: calc(100% - 20px);
  min-height: 40px;
  margin: 0 auto;
  background-color: #002B36;
  position: relative;
  button {
    position: absolute;
    right: 10px;
  }
}
</style>
