<template>
  <button :class="mode">
    <slot></slot>
  </button>
</template>

<script>
export default {
  //we define a props that is used as a class name with the '.flat' selector (--> TheRecipes.vue)
  //see also template above <button :class="mode">
  props: ['mode'],
};
</script>

<style scoped>
button {
  padding: 0.75rem 1.5rem;
  font-family: inherit;
  background-color: #e4cda7;
  border: 1px solid #e4cda7;
  color: #8e806a;
  cursor: pointer;
  border-radius: 10px;
  font-weight: 700;
}

button:hover,
button:active {
  background-color: #ffe6bc;
  border-color: #ffe6bc;
}

.flat {
  background-color: transparent;
  color: #8e806a;
  border: none;
  border-radius: 10px;
}

.flat:hover,
.flat:active {
  background-color: #ffe6bc;
}
</style>
