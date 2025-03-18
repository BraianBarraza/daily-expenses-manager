<script setup>
import closeModal from '../assets/img/cerrar.svg'

const emit = defineEmits(['hide-modal'])
const props = defineProps({
  modal:{
    type:Object,
    required:true
  }
})

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
      >
        <legend>Add a new expense</legend>

        <div class="field">
          <label for="name">Expense Name:</label>
          <input type="text"
                 id="name"
                 placeholder="Add the Expense Name">
        </div>
        <div class="field">
          <label for="Quantity">Expense Quantity:</label>
          <input type="text"
                 id="Quantity"
                 placeholder="Quantity of the expense, Ex. 300">
        </div>
        <div class="field">
          <label for="Category">Expense Category:</label>
          <select id="Category">
            <option value="saving">Money Saving</option>
            <option value="food">Food</option>
            <option value="home">Home</option>
            <option value="health">Health</option>
            <option value="entertainment">Entertainment</option>
            <option value="clothe">Clothe</option>
            <option value="subscriptions">Subscriptions</option>
            <option value="others">Others</option>
          </select>
        </div>

        <input
            type="submit"
            value="Add a new Expense"
        />


      </form>

    </div>
    <h1>modal</h1>
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