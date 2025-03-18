<script setup>
import {ref, reactive} from "vue";
import Budget from "./components/Budget.vue";
import BudgetControl from "./components/BudgetControl.vue";
import Modal from "./components/Modal.vue";

import iconNewExpense from './assets/img/nuevo-gasto.svg'

const modal = reactive({
  show: false,
  animate: false
})

const budget = ref(0)
const available = ref(0)

const defineBudget = (quantity) => {
  budget.value = quantity
  available.value = quantity
}

const showModal = () => {
  modal.show = true
  setTimeout(()=>{
  modal.animate = true
  },300)
}
const hideModal = () => {
    modal.animate = false
  setTimeout(()=>{
    modal.show = false
  },300)
}
</script>

<template>
  <div>
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
        />


      </div>

    </header>

    <main v-if="budget > 0">
      <div class="new-expense">
        <img :src="iconNewExpense" alt="New Expense Icon"
             @click="showModal">
      </div>

      <Modal
      v-if="modal.show"
      @hide-modal = "hideModal"
      :modal="modal"
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

.new-expense {
  position: fixed;
  bottom: 5rem;
  right: 5rem;
}

.new-expense img {
  width: 5rem;
  cursor: pointer;
}

</style>