<template>
  <!-- this is the default page, with the selection of menu buttons -->
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

  <!-- keep-alive is a wrapper for a dynamic component, to keep the conent alive. 
  Otherwise it would get deleted from DOM with every switch between components -->
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredRecipes from './StoredRecipes.vue';
import AddRecipe from './AddRecipe.vue';

export default {
  components: {
    StoredRecipes,
    AddRecipe,
  },

  data() {
    return {
      //selectedTab gets a default value of the first button 'stored-recipes'
      selectedTab: 'stored-recipes',

      storedRecipes: [
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
      ],
    };
  },

  provide() {
    return {
      //this is what we use to inject in other files
      recipes: this.storedRecipes,
      addRecipe: this.addRecipe,
      deleteRecipe: this.removeRecipe,
    };
  },

  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-recipes' ? null : 'flat';
    },

    addResButtonMode() {
      return this.selectedTab === 'add-recipe' ? null : 'flat';
    },
  },

  methods: {
    //at click of each menu button, we switch the value of selectedTab. This function receives an parameter for his argument, provided at click of the buttons above
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },

    //here we create a new object with the input fields rom the user
    addRecipe(title, description, url, img) {
      const newRecipe = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url,
        img: img,
      };

      //and push it to the array of objects 'storedRecipes' with unshift. In goes in front of the array, as the first element
      this.storedRecipes.unshift(newRecipe);

      //'selectedTab' is restored to it's initial value
      this.selectedTab = 'stored-recipes';
    },

    removeRecipe(resId) {
      //this functon receives the id of the object to be deleted, as a parameter.
      //It searched for the element with the same id and deletes it from the array with the splice method.
      //Only one element gets deleted.
      const resIndex = this.storedRecipes.findIndex((res) => res.id === resId);
      this.storedRecipes.splice(resIndex, 1);
    },
  },
};
</script>
