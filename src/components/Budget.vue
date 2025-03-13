<script setup>
import {ref} from "vue";
import Alert from "./Alert.vue";

const budget = ref(0)
const error = ref('')

const emit = defineEmits(['define-budget'])
const defineBudget = ()=>{
  if (budget.value <= 0){
    error.value= 'Invalid Budget'
    setTimeout(()=>{
      error.value = ''
    }, 3000)

    return
  }
  emit('define-budget', budget.value)
}
</script>

<template>
  <form
  class="budget"
  @submit.prevent="defineBudget"
  >
    <Alert v-if="error">
      {{ error }}
    </Alert>

    <div class="field">
      <label for="">Define Your Budget</label>

      <input
          id="new-budget"
          class="new-budget"
          placeholder="Add your budget"
          type="number"
          v-model.number="budget">
    </div>

    <input type="submit" value="Define your Budget">
  </form>

</template>

<style scoped>
.budget{
  width: 100%;
}
.field{
  display: grid;
  gap: 2rem;
}
.budget label{
  font-size: 2rem;
  color: var(--blue);
  text-align: center;
}
.budget input[type="number"]{
  background-color: var(--soft-grey);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
  text-align: center;
}

.budget input[type="submit"]{
  background-color: var(--blue);
  border: none;
  border-radius: 1rem;
  padding: 1rem;
  font-size: 2rem;
  text-align: center;
  margin-top: 2rem;
  color: var(--white);
  font-weight: 800;
  width: 100%;
  transition: background-color 0.3s ease;
  &:hover{
    background-color: #1048a4;
    cursor: pointer;
  }
}

</style>