
<template>
<div class="alert">
  <slot></slot>
</div>
</template>

<style scoped>
.alert{
  padding: 1rem;
  font-weight: 980;
  text-transform: uppercase;
  font-size: 1.6rem;
  text-align: center;
  background-color: var(--white);
  border-left:.5rem solid #b91c1c;
  color: #b91c1c;
  margin-bottom: 2rem;
}
</style>