<template>
  <ul>
    <!-- we use here the props provided in LearningRecipe.vue and we loop through the injected 'recipes' array of objects and bind each key: value to it-->
    <learning-recipe
      v-for="res in recipes"
      :key="res.id"
      :id="res.id"
      :title="res.title"
      :description="res.description"
      :link="res.link"
      :img="res.img"
    ></learning-recipe>
  </ul>
</template>

<script>
import LearningRecipe from './LearningRecipe.vue';

export default {
  //the recipe array is injected here. It is provided from TheRecipes.vue file
  inject: ['recipes'],

  components: {
    LearningRecipe,
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
  margin: auto;
  max-width: 40rem;
}
</style>
