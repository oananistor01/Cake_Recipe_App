<template>
  <!-- this is the default page, with the selection of menu buttons/tabs -->
  <!-- :mode is a props sent from BaseButton.vue, where <button :class="mode"> is used -->
  <base-card>
    <base-button
      @click="setSelectedTab('stored-recipes')"
      :mode="storedResButtonMode"
      >Stored Recipes</base-button
    >
    <base-button @click="setSelectedTab('add-recipe')" :mode="addResButtonMode"
      >Add Recipe</base-button
    >
  </base-card>

  <!-- keep-alive is a wrapper for a dynamic component, to keep the content alive. 
  Otherwise it would get deleted from DOM with every switch between components -->
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import { ref, computed, provide } from 'vue';
import StoredRecipes from './StoredRecipes.vue';
import AddRecipe from './AddRecipe.vue';

export default {
  components: {
    StoredRecipes,
    AddRecipe,
  },

  setup() {
    const selectedTab = ref('stored-recipes'); //selectedTab gets a default value of the first button 'stored-recipes'
    const storedRecipes = ref([
      {
        id: 'vanilla-cake',
        title: 'Vanilla Cake',
        description:
          'With its outstanding vanilla flavor, pillowy soft crumb, and creamy vanilla buttercream, this is truly the best vanilla cake.',
        link: 'https://sallysbakingaddiction.com/vanilla-cake/',
        img: 'https://cdn.sallysbakingaddiction.com/wp-content/uploads/2019/01/vanilla-cake-600x900.jpg',
      },
      {
        id: 'strawberry-cake',
        title: 'Strawberry Cake',
        description:
          'The one thing that sets this strawberry cake apart from others? Reduce fresh strawberry puree down and add to the best white cake batter.',
        link: 'https://sallysbakingaddiction.com/strawberry-cake/',
        img: 'https://cdn.sallysbakingaddiction.com/wp-content/uploads/2017/12/homemade-strawberry-cake-4-600x900.jpg',
      },
    ]);

    //this is what we use to inject in other files
    provide('addRecipe', addRecipe);
    provide('recipes', storedRecipes);
    provide('deleteRecipe', removeRecipe);

    //when one button is active, it changes the value of selectedTab and sets a class name to 'flat'
    const storedResButtonMode = computed(function () {
      return selectedTab.value === 'stored-recipes' ? null : 'flat';
    });

    const addResButtonMode = computed(function () {
      return selectedTab.value === 'add-recipe' ? null : 'flat';
    });

    //at click of each menu button, we switch the value of selectedTab. This function receives a parameter for his argument, provided at click of the buttons above
    function setSelectedTab(tab) {
      selectedTab.value = tab;
    }

    //here we create a new object with the input fields from the user
    function addRecipe(title, description, url, img) {
      const newRecipe = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url,
        img: img,
      };

      storedRecipes.value.unshift(newRecipe); //and push it to the array of objects 'storedRecipes' with unshift. In goes in front of the array, as the first element
      console.log(newRecipe);

      selectedTab.value = 'stored-recipes'; //'selectedTab' is restored to it's initial value, so it switches to the default tab
    }

    function removeRecipe(recipeId) {
      //this functon receives from RecipeCard.vue, the id of the object to be deleted, as a parameter.
      const recipeIndex = storedRecipes.value.findIndex(
        (recipe) => recipe.id === recipeId
      ); //It searches for the element with the same id and deletes it from the array with the splice method.

      storedRecipes.value.splice(recipeIndex, 1); //Only one element gets deleted from the array.
    }

    return {
      selectedTab,
      storedResButtonMode,
      addResButtonMode,
      setSelectedTab,
    };
  },

  /******** THE OPTIONS API ********/

  // provide() {
  //   return {
  //     recipes: this.storedRecipes,
  //     deleteRecipe: this.removeRecipe,
  //   };
  // },

  // data() {
  //   return {
  //     selectedTab: 'stored-recipes',

  //     storedRecipes: [
  //       {
  //         id: 'vanilla-cake',
  //         title: 'Vanilla Cake',
  //         description:
  //           'With its outstanding vanilla flavor, pillowy soft crumb, and creamy vanilla buttercream, this is truly the best vanilla cake.',
  //         link: 'https://sallysbakingaddiction.com/vanilla-cake/',
  //         img: 'https://cdn.sallysbakingaddiction.com/wp-content/uploads/2019/01/vanilla-cake-600x900.jpg',
  //       },
  //       {
  //         id: 'strawberry-cake',
  //         title: 'Strawberry Cake',
  //         description:
  //           'The one thing that sets this strawberry cake apart from others? Reduce fresh strawberry puree down and add to the best white cake batter.',
  //         link: 'https://sallysbakingaddiction.com/strawberry-cake/',
  //         img: 'https://cdn.sallysbakingaddiction.com/wp-content/uploads/2017/12/homemade-strawberry-cake-4-600x900.jpg',
  //       },
  //     ],
  //   };
  // },

  // computed: {
  //   storedResButtonMode() {
  //     return this.selectedTab === 'stored-recipes' ? null : 'flat';
  //   },

  //   addResButtonMode() {
  //     return this.selectedTab === 'add-recipe' ? null : 'flat';
  //   },
  // },

  // methods: {
  //   setSelectedTab(tab) {
  //     this.selectedTab = tab;
  //   },

  //   addRecipe(title, description, url, img) {
  //     const newRecipe = {
  //       id: new Date().toISOString(),
  //       title: title,
  //       description: description,
  //       link: url,
  //       img: img,
  //     };

  //     this.storedRecipes.unshift(newRecipe);
  //     this.selectedTab = 'stored-recipes';
  //   },

  //   removeRecipe(recipeId) {
  //     const recipeIndex = this.storedRecipes.findIndex((recipe) => recipe.id === recipeId);
  //     this.storedRecipes.splice(recipeIndex, 1);
  //   },
  // },
};
</script>
