<template>
  <el-card :header="header">
    <template v-if="!isEmpty">
      <app-budget-item
        v-for="budget in budgets"
        :budget="budget"
        :key="budget.id"
        @deleteItem="$emit('deleteItem', $event)"
      />
    </template>
    <el-alert v-else type="info" :title="emptyTitle" :closable="false" />
  </el-card>
</template>

<script>
import AppBudgetItem from '@/components/BudgetItem';

export default {
  name: 'AppBudgetList',
  props: {
    budgets: {
      type: Array,
      default: () => []
    }
  },
  data: () => ({
    header: 'Budget List',
    emptyTitle: 'Empty List'
  }),
  components: {
    AppBudgetItem
  },
  computed: {
    isEmpty () {
      return !this.budgets.length;
    }
  }
};
</script>
