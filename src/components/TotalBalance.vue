<template>
  <div :class="['total-balance', color]">Balance: {{ total.toLocaleString() }}</div>
</template>

<script>
export default {
  name: 'AppTotalBalance',
  props: {
    total: {
      type: Number,
      default: 0
    }
  },
  computed: {
    color () {
      if (this.total) {
        return this.total > 0 ? 'text-success' : 'text-danger';
      }

      return '';
    }
  }
}
</script>

<style scoped lang="scss">
.total-balance {
  padding: 30px;
  font-size: 30px;
  text-transform: uppercase;
  text-align: center;
}
</style>
