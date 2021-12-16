<template>
  <the-header title="Cake Recipes"></the-header>
  <the-recipes></the-recipes>
</template>

<script>
import TheHeader from './components/layouts/TheHeader.vue';
import TheRecipes from './components/learning-recipes/TheRecipes.vue';

export default {
  components: {
    TheHeader,
    TheRecipes,
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

* {
  box-sizing: border-box;
}

html {
  font-family: 'Roboto', sans-serif;
}

body {
  margin: 0;
}
</style>
