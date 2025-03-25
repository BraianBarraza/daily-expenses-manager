<script setup>
import {ref} from "vue";
import closeModal from '../assets/img/close.svg'
import Alert from "./Alert.vue";

const error = ref('')
const emit = defineEmits(['hide-modal', 'save-new-expense', 'update:name', 'update:quantity', 'update:category',
  'delete-expense'])
const props = defineProps({
  modal: {
    type: Object,
    required: true
  },
  name: {
    type: String,
    required: true
  },
  quantity: {
    type: [String, Number],
    required: true
  },
  category: {
    type: String,
    required: true
  },
  id: {
    type: [String, null],
    required: true
  }
})

const addNewExpense = () => {
  //no empty fields validation
  const {name, quantity, category} = props
  if ([name, quantity, category].includes("")) {
    error.value = 'all fields must be filled'
    setTimeout(() => {
      error.value = ''
    }, 4000)
    return
  }

  //quantity validation
  if ([quantity] <= 0) {
    error.value = 'Please introduce a valid Quantity'
    setTimeout(() => {
      error.value = ''
    }, 4000)
    return;
  }

  emit("save-new-expense")

}

const isEditing = () => props.id !== null

</script>

<template>
  <div class="modal">
    <div class="close-modal">
      <img :src="closeModal"
           alt="Close Modal"
           @click="$event => $emit('hide-modal')"
      />
    </div>

    <div class="container form-container"
         :class="[modal.animate ? 'animate' : 'close']">
      <form
          class="new-expense"
          @submit.prevent="addNewExpense"
      >
        <legend>{{ isEditing() ? 'Edit Expense: ' : 'Add a new expense' }}</legend>

        <Alert v-if="error">{{ error }}</Alert>

        <div class="field">
          <label for="name">Expense Name:</label>
          <input type="text"
                 id="name"
                 placeholder="Add the Expense Name"
                 :value="name"
                 @input="$emit('update:name', $event.target.value)">
        </div>
        <div class="field">
          <label for="Quantity">Expense Quantity:</label>
          <input type="text"
                 id="Quantity"
                 placeholder="Quantity of the expense, Ex. 300"
                 :value="quantity"
                 @input="$emit('update:quantity', +$event.target.value)">
        </div>
        <div class="field">
          <label for="Category">Expense Category:</label>
          <select id="Category"

                  :value="category"
                  @input="$emit('update:category', $event.target.value)">

            <option value="">-- Select --</option>
            <option value="save">Money Saving</option>
            <option value="food">Food</option>
            <option value="home">Home</option>
            <option value="health">Health</option>
            <option value="entertainment">Entertainment</option>
            <option value="clothes">Clothes</option>
            <option value="subscriptions">Subscriptions</option>
            <option value="others">Others</option>
          </select>
        </div>

        <input
            type="submit"
            :value="[isEditing() ? 'Save changes' : 'Add a new expense']"
        />


      </form>

      <button
        type="button"
        class="delete-btn"
        v-if="isEditing()"
        @click="$emit('delete-expense')"
      >
        Delete Expense
      </button>

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

.new-expense {
  margin: 10rem auto 0 auto;
  display: grid;
  gap: 2rem;
}

.new-expense legend {
  text-align: center;
  color: var(--white);
  font-size: 3rem;
  font-weight: 700;
}

.field {
  display: grid;
  gap: 2rem;
}

.new-expense input,
.new-expense select {
  background-color: var(--soft-grey);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
}

.new-expense label {
  color: var(--white);
  font-size: 3rem;
}

.new-expense input[type="submit"] {
  background-color: var(--blue);
  color: var(--white);
  font-weight: 700;
  cursor: pointer;
}

.delete-btn {
  border: none;
  padding: 1rem;
  width: 100%;
  background-color: #ef4444;
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--white);
  margin-top: 10rem;
  cursor: pointer;
}
</style>