<template>
  <ul>
    <!-- we use here the props provided in RecipeCard.vue and we loop through the injected 'recipes' array of objects and bind each key: value to it-->
    <recipe-card
      v-for="res in recipes"
      :key="res.id"
      :id="res.id"
      :title="res.title"
      :description="res.description"
      :link="res.link"
      :img="res.img"
    ></recipe-card>
  </ul>
</template>

<script>
import { inject } from 'vue';
import RecipeCard from './RecipeCard.vue';

export default {
  components: {
    RecipeCard,
  },

  setup() {
    const recipes = inject('recipes'); //the recipe array is injected here. It is provided from TheRecipes.vue file

    return {
      recipes,
    };
  },

  /******** THE OPTIONS API ********/
  //inject: ['recipes'],
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
