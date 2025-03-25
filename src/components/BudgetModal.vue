<!-- src/components/BudgetModal.vue -->

<script setup>
import { ref } from 'vue'
import closeModal from '../assets/img/close.svg'
import Alert from './Alert.vue'

const props = defineProps({
  modal: {
    type: Object,
    required: true
  },
  currentBudget: {
    type: Number,
    required: true
  }
})
const emit = defineEmits(['hide-modal', 'update-budget'])

const error = ref('')
const newBudget = ref(props.currentBudget)

const saveNewBudget = () => {
  if (newBudget.value <= 0) {
    error.value = 'Please introduce a valid Budget'
    setTimeout(() => {
      error.value = ''
    }, 4000)
    return
  }
  emit('update-budget', newBudget.value)
  emit('hide-modal')
}
</script>

<template>
  <div class="modal">
    <div class="close-modal">
      <img
          :src="closeModal"
          alt="Close Modal"
          @click="$emit('hide-modal')"
      />
    </div>

    <div
        class="container form-container"
        :class="[modal.animate ? 'animate' : 'close']"
    >
      <form class="new-budget" @submit.prevent="saveNewBudget">
        <legend>Edit Budget</legend>

        <Alert v-if="error">
          {{ error }}
        </Alert>

        <div class="field">
          <label for="budget">New Budget:</label>
          <input
              type="number"
              id="budget"
              placeholder="Add the new Budget"
              v-model="newBudget"
          />
        </div>

        <input
            type="submit"
            value="Save changes"
        />
      </form>
    </div>
  </div>
</template>

<style scoped>
.modal {
  position: absolute;
  background-color: rgb(0 0 0 / 0.9);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.close-modal {
  position: absolute;
  right: 3rem;
  top: 3rem;
}

.close-modal img {
  width: 3rem;
  cursor: pointer;
}

.form-container {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}

.form-container.animate {
  opacity: 1;
}

.form-container.close {
  opacity: 0;
}

.new-budget {
  margin: 10rem auto 0 auto;
  display: grid;
  gap: 2rem;
}

.new-budget legend {
  text-align: center;
  color: var(--white);
  font-size: 3rem;
  font-weight: 700;
}

.field {
  display: grid;
  gap: 2rem;
}

.new-budget input,
.new-budget select {
  background-color: var(--soft-grey);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
}

.new-budget label {
  color: var(--white);
  font-size: 3rem;
}

.new-budget input[type='submit'] {
  background-color: var(--blue);
  color: var(--white);
  font-weight: 700;
  cursor: pointer;
}
</style>
