<script setup>
import {reactive, computed} from 'vue'
import {formatQuantity} from '../helpers'
import BudgetModal from './BudgetModal.vue'

import "vue3-circle-progress/dist/circle-progress.css";
import CircleProgress from "vue3-circle-progress";

const props = defineProps({
  budget: {
    type: Number,
    required: true
  },
  available: {
    type: Number,
    required: true
  },
  spent: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['reset-app', 'modify-budget'])

const budgetModal = reactive({
  show: false,
  animate: false
})

const showBudgetModal = () => {
  budgetModal.show = true
  setTimeout(() => {
    budgetModal.animate = true
  }, 300)
}

const hideBudgetModal = () => {
  budgetModal.animate = false
  setTimeout(() => {
    budgetModal.show = false
  }, 300)
}

const updateBudget = (newBudget) => {
  emit('modify-budget', newBudget)
}

const percentage = computed(() => {
  return parseInt(((props.budget - props.available) / props.budget) * 100)
})
</script>

<template>
  <div class="two-columns">
    <div class="graphic-container">
      <p class="percentage" :style="[percentage > 100 ? {'color':'red'} : {'color':'green'}]">
        {{percentage}}%
      </p>
      <CircleProgress
          :percent="percentage"
          :size="250"
          :border-width="30"
          :border-bg-width="30"
          fill-color="#3b82f6"
          empty-color="#e1e1e1"
      />
    </div>

    <div class="budget-container">
      <div class="two-columns">
        <button
            class="reset-app"
            @click="showBudgetModal"
        >
          Modify Budget
        </button>

        <button
            class="reset-app"
            @click="$event=> $emit('reset-app')"
        >
          Reset App
        </button>
      </div>
      <p>
        <span>Total Budget:</span>
        {{ formatQuantity(budget) }}
      </p>

      <p :style="[available > 0 ? {'color':'green', 'font-weight': 'bold'} : {'color':'red', 'font-weight': 'bold'}]">
        <span>Available Budget:
        </span>
        {{ formatQuantity(available) }}
      </p>

      <p>
        <span>Spent Budget:
        </span>
        {{ formatQuantity(spent) }}
      </p>
    </div>
  </div>

  <BudgetModal
      v-if="budgetModal.show"
      :modal="budgetModal"
      :currentBudget="budget"
      @hide-modal="hideBudgetModal"
      @update-budget="updateBudget"
  />

</template>

<style scoped>
.graphic-container {
  position: relative;
}

.percentage {
  position: absolute;
  margin: auto;
  top: calc(50% - 1.5rem);
  left: 0;
  right: 0;
  text-align: center;
  z-index: 100;
  font-size: 3rem;
  font-weight: 900;
}

.two-columns {
  display: flex;
  flex-direction: column;
}

.two-columns > :first-child {
  margin-bottom: 3rem;
}

@media (min-width: 768px) {
  .two-columns {
    flex-direction: row;
    gap: 4rem;
    align-items: center;
  }

  .two-columns > :first-child {
    margin-bottom: 0;
  }
}

.reset-app {
  background-color: #DB2777;
  border: none;
  padding: 1rem;
  width: 100%;
  color: var(--white);
  font-weight: 900;
  text-transform: uppercase;
  border-radius: 1rem;
  transition-property: background-color;
  transition-duration: 300ms;
}

.reset-app:hover {
  cursor: pointer;
  background-color: #c11d67;
}

.budget-container {
  width: 100%;
}

.budget-container p {
  font-size: 2.4rem;
  text-align: center;
  color: var(--dark-grey);
}

@media (min-width: 768px) {
  .budget-container p {
    text-align: left;
  }
}

.budget-container span {
  font-weight: 900;
  color: var(--blue);
}
</style>