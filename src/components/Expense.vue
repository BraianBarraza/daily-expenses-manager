<script setup>

const props = defineProps({
  expense: {
    type: Object,
    required: true
  }
})
</script>

<template>

  <div class="expense shadow">
    <div class="content">

    </div>

    <p>{{expense.quantity}}</p>
  </div>

</template>

<style scoped>

</style>