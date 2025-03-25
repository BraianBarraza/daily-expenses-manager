<script setup>
import {ref, reactive, watch, computed, onMounted} from "vue";
import Budget from "./components/Budget.vue";
import BudgetControl from "./components/BudgetControl.vue";
import Modal from "./components/Modal.vue";
import Expense from "./components/Expense.vue";
import Filters from "./components/Filters.vue";

import {generateId} from "./helpers/index.js";
import iconNewExpense from './assets/img/new_expense.svg'

const modal = reactive({
  show: false,
  animate: false
})

const budget = ref(0)
const available = ref(0)
const spent = ref(0)
const filter = ref('')

const expense = reactive({
  name: '',
  quantity: '',
  category: '',
  id: null,
  date: Date.now(),
})

const expenses = ref([])

watch(expenses, () => {
  const totalSpent = expenses.value.reduce((total, expense) => expense.quantity + total, 0)
  spent.value = totalSpent
  available.value = budget.value - totalSpent

  localStorage.setItem('expenses', JSON.stringify(expenses.value))
}, {
  deep: true
})

watch(modal, () => {
  if (!modal.show) {
    restartExpenseState()
  }
}, {
  deep: true
})

watch(budget, () => {
  localStorage.setItem('budget', budget.value)
})

onMounted(() => {
  const storageBudget = localStorage.getItem('budget')
  if (storageBudget) {
    budget.value = Number(storageBudget)
    available.value = Number(storageBudget)
  }

  const expensesStorage = localStorage.getItem('expenses')
  if (expensesStorage) {
    expenses.value = JSON.parse(expensesStorage)
  }
})

const defineBudget = (quantity) => {
  budget.value = quantity
  available.value = quantity
}

const showModal = () => {
  modal.show = true
  setTimeout(() => {
    modal.animate = true
  }, 300)
}
const hideModal = () => {
  modal.animate = false
  setTimeout(() => {
    modal.show = false
  }, 300)
}

const saveNewExpense = () => {
  if (expense.id === null) {
    expenses.value.push({
      ...expense,
      id: generateId()
    })
  } else {
    const index = expenses.value.findIndex(item => item.id === expense.id)
    if (index !== -1) {
      expenses.value[index] = {...expense}
    }
  }

  hideModal()

  restartExpenseState()
}

const restartExpenseState = () => {
  //empty fields
  Object.assign(expense, {
    name: '',
    quantity: '',
    category: '',
    id: null,
    date: Date.now(),
  })
}

const selectExpense = id => {
  const editExpense = expenses.value.filter(expense => expense.id === id)[0]
  Object.assign(expense, editExpense)
  showModal()
}

const deleteExpense = () => {
  if (confirm('Do you want to delete "' + expense.name + '"?')) {
    expenses.value = expenses.value.filter(expenseSate => expenseSate.id !== expense.id)
    hideModal()
  }
}

const modifyBudget = (newBudget) =>{
  budget.value = newBudget
  available.value = newBudget - spent.value
}

const resetApp = () => {
  if (confirm('Sure you want to reset all the values?')){
    budget.value = 0
    expenses.value = []
  }
}

const filterExpenses = computed(() => {
  if (filter.value) {
    return expenses.value.filter(expense => expense.category === filter.value)
  }
  return expenses.value
})
</script>

<template>
  <div
      :class="{fix: modal.show}"
  >
    <header>
      <h1>Daily Expenses Manager</h1>
      <div class="header-container container shadow">
        <Budget
            v-if="budget === 0"
            @define-budget="defineBudget"
        />
        <BudgetControl
            v-else
            :budget="budget"
            :available="available"
            :spent="spent"
            @modify-budget = "modifyBudget"
            @reset-app="resetApp"
        />


      </div>

    </header>

    <main v-if="budget > 0">

      <Filters
          v-model:filter="filter"
      />

      <div class="expenses-list container">

        <h2>{{ filterExpenses.length > 0 ? 'My Expenses.' : 'There are no Expenses.' }}</h2>

        <Expense
            v-for="expense in filterExpenses"
            :key="expense.id"
            :expense="expense"
            @select-expense="selectExpense"
        />

      </div>

      <div class="create-expense">
        <img :src="iconNewExpense" alt="New Expense Icon"
             @click="showModal">
      </div>

      <Modal
          v-if="modal.show"
          @hide-modal="hideModal"
          @save-new-expense="saveNewExpense"
          @delete-expense="deleteExpense"
          :modal="modal"
          v-model:name="expense.name"
          v-model:quantity="expense.quantity"
          v-model:category="expense.category"
          v-model:id="expense.id"
      />
    </main>
  </div>

</template>

<style>
:root {
  --blue: #3b82f6;
  --white: #fff;
  --soft-grey: #f5f5f5;
  --grey: #94a3b;
  --dark-grey: #64748b;
  --black: #000;
}

html {
  font-size: 62.5%;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
  background-color: var(--soft-grey);
}

h1 {
  font-size: 4rem;
}

h2 {
  font-size: 3rem;
}

.fix {
  overflow: hidden;
  height: 100vh;
}

header {
  background-color: var(--blue);
}

header h1 {
  padding: 3rem;
  margin: 0;
  color: var(--white);
  text-align: center;
}

.container {
  width: 90%;
  max-width: 80rem;
  margin: 0 auto;
}

.header-container {
  margin-top: -5rem;
  transform: translateY(5rem);
  padding: 5rem;

}

.shadow {
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  background-color: var(--white);
  border-radius: 1.2rem;
  padding: 5rem;

}

.create-expense {
  position: fixed;
  bottom: 5rem;
  right: 5rem;
}

.create-expense img {
  width: 5rem;
  cursor: pointer;
}

.expenses-list {
  margin-top: 10rem;
}

.expenses-list h2 {
  font-weight: 900;
  color: var(--dark-grey);
}

</style>