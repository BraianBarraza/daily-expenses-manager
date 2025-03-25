<script setup>
import {formatQuantity} from "../helpers/index.js";
import {dateFormatter} from "../helpers/index.js";

import saveIcon from '../assets/img/save_icon.svg'
import homeIcon from '../assets/img/home_icon.svg'
import foodIcon from '../assets/img/food_icon.svg'
import expensesIcon from '../assets/img/expenses_icon.svg'
import entertainmentIcon from '../assets/img/entertainment_icon.svg'
import healthIcon from '../assets/img/health_icon.svg'
import subscriptionsIcon from '../assets/img/subscriptions-icon.svg'

const dictionaryIcons = {
  save: saveIcon,
  food: foodIcon,
  home: homeIcon,
  expenses: expensesIcon,
  entertainment: entertainmentIcon,
  health: healthIcon,
  subscriptions: subscriptionsIcon
}

const props = defineProps({
  expense: {
    type: Object,
    required: true
  }
})

defineEmits(['select-expense'])
</script>

<template>

  <div class="expense shadow">
    <div class="content">
      <img :src="dictionaryIcons[expense.category]"
           alt="Expenses Icon"
           class="icon"
      >

      <div class="details">
        <p class="category">{{ expense.category }}</p>
        <p
            class="name"
            @click="$emit('select-expense', expense.id)"
        >
          {{ expense.name }}
        </p>

        <p class="date">{{ dateFormatter(expense.date) }}</p>
      </div>
    </div>

    <p class="quantity">{{ formatQuantity(expense.quantity) }}</p>
  </div>

</template>

<style scoped>
.expense {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.content {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.icon {
  width: 5rem;
}

.details p {
  margin: 0 0 1rem 0;
}

.category {
  color: var(--grey);
  font-size: 1.2rem;
  text-transform: uppercase;
  font-weight: 900;
}

.name {
  color: var(--dark-grey);
  font-size: 2.4rem;
  font-weight: 700;
  cursor: pointer;
}

.date {
  color: var(--dark-grey);
  font-size: 1.6rem;
  font-weight: 900;
}

.date span {
  font-weight: 400;
}

.quantity {
  font-size: 3rem;
  font-weight: 900;
  margin: 0;
}
</style>