<script setup>
import image from '../assets/img/grafico.jpg'
import {formatQuantity} from "../helpers"

const props = defineProps({
  budget: {
    type: Number,
    required: true,
  },
  available: {
    type: Number,
    required: true,
  },
  spent: {
    type: Number,
    required: true,
  }

})

defineEmits(['reset-budget'])

</script>

<template>
  <div class="two-columns">
    <div class="graphic-container">
      <img :src="image"
           alt=""
      />

    </div>

    <div class="budget-container">
      <button
          class="reset-app"
          @click="$event=> $emit('resetBudget')"
      >
        Reset Budget
      </button>
      <p>
        <span>Total Budget:</span>
        {{ formatQuantity(budget) }}
      </p>

      <p :style="[available > 0 ? {'color':'green'} : {'color':'red', 'font-weight': 'bold'}]">
        <span>Available Budget:
        </span>
        {{ formatQuantity(available) }}
      </p>

      <p>
        <span>Spent Budget:
        </span>
        {{formatQuantity(spent)}}
      </p>
    </div>
  </div>

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
  color: var(--dark-grey);
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