<template>
  <li>
    <!-- here is every recipe card, with the title, the delete button, the description, the picture and the link to the recipe -->
    <base-card>
      <header>
        <h3>{{ title }}</h3>
        <base-button mode="flat" @click="deleteRecipe(id)">Delete</base-button>
      </header>

      <div class="desc-img">
        <img :src="img" alt="" style="max-width: 100px" />
        <p>{{ description }}</p>
      </div>

      <nav>
        <a :href="link" target="_blank">View Recipe</a>
      </nav>
    </base-card>
  </li>
</template>

<script>
import { inject } from 'vue';

export default {
  props: ['id', 'title', 'description', 'link', 'img'], //the props that we are using in StoredRecipes.vue

  setup() {
    const deleteRecipe = inject('deleteRecipe'); //inject deleteRecipe method from TheRecipes.vue

    return {
      deleteRecipe,
    };
  },

  /******** THE OPTIONS API ********/
  //inject: ['deleteRecipe'],
};
</script>

<style scoped>
li {
  margin: auto;
  max-width: 40rem;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h3 {
  font-size: 1.25rem;
  margin: 0.5rem 0;
}

.desc-img {
  display: flex;
  align-items: center;
}

p,
img {
  margin: 0.5rem 0;
  font-size: 12px;
  padding: 0 20px 0 0;
}

a {
  text-decoration: none;
  color: #ce5c00;
  font-size: smaller;
}

a:hover,
a:active {
  color: #c89300;
}
</style>
