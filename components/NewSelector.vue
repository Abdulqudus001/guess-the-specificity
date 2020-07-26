<template>
  <div class="selector">

  </div>
</template>

<script>
import classes from '~/db/classes';
import ids from '~/db/ids';

export default {
  mounted() {
    console.log(ids);
    console.log(classes);
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
