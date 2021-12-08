<template>
  <div class="budget-item">
    <span class="budget-item__icon"><i :class="icon"></i></span>
    <span class="budget-item__comment">{{ budget.comment }}</span>
    <span class="budget-item__value">{{ budget.value.toLocaleString() }}</span>
    <el-button type="danger" size="mini" @click="deleteItem(budget.id)">Delete</el-button>
  </div>
</template>

<script>
export default {
  name: 'AppBudgetItem',
  props: {
    budget: {
      type: Object,
      required: true
    }
  },
  computed: {
    icon () {
      return this.budget.type === 'INCOME'
        ? 'el-icon-caret-top text-success'
        : 'el-icon-caret-bottom text-danger';
    }
  },
  methods: {
    deleteItem (id) {
      this.$emit('deleteItem', id);
    }
  }
}
</script>

<style scoped lang="scss">
.budget-item {
  display: flex;
  align-items: center;
  padding: 10px 15px;

  &__icon {
    margin-right: 20px;
  }

  &__value {
    font-weight: bold;
    margin-left: auto;
    margin-right: 20px;
  }
}
</style>
