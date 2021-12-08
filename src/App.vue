<template>
  <div id="app">
    <app-form @submitForm="onSubmit" />
    <app-total-balance :total="totalBalance" />
    <app-budget-list :budgets="budgets" @deleteItem="onDeleteItem" />
  </div>
</template>

<script>
import AppForm from '@/components/Form';
import AppTotalBalance from '@/components/TotalBalance';
import AppBudgetList from '@/components/BudgetList';

const BACKEND_URL = 'http://localhost:5000';

export default {
  name: 'App',
  data: () => ({
    budgets: []
  }),
  components: {
    AppForm,
    AppTotalBalance,
    AppBudgetList
  },
  computed: {
    totalBalance () {
      return this.budgets.reduce((accum, item) => accum + item.value, 0);
    }
  },
  methods: {
    async fetchList () {
      try {
        const response = await fetch(`${BACKEND_URL}/budgets`);
        const data = await response.json();

        this.budgets = [...data];
      } catch (error) {
        console.log(error);
      }
    },
    async onSubmit (budget) {
      try {
        const response = await fetch(`${BACKEND_URL}/budgets`, {
          method: 'POST',
          headers: {
            'Content-type': 'application/json'
          },
          body: JSON.stringify(budget)
        });
        const data = await response.json();

        this.budgets.push(data);
      } catch (error) {
        console.log(error);
      }
    },
    async onDeleteItem (id) {
      if (confirm('Are you sure you want to delete?')) {
        try {
          await fetch(`${BACKEND_URL}/budgets/${id}`, { method: 'DELETE' });

          this.budgets = this.budgets.filter(item => item.id !== id);
        } catch (error) {
          console.log(error);
        }
      }
    }
  },
  mounted () {
    this.fetchList();
  }
}
</script>

<style lang="scss">
body {
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#app {
  max-width: 600px;
  margin: 60px auto;
  text-align: center;
}

.text-success {
  color: #67C23A;
}

.text-danger {
  color: #F56C6C;
}
</style>
