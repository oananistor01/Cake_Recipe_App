<template>
  <!-- here comes the error message displayed when input is invalid -->
  <base-dialog
    v-if="inputIsInvalid"
    title="Invalid Input"
    @close="confirmError"
  >
    <!-- this is the default slot -->
    <template #default>
      <p>Unfortunately, at least one input value is invalid.</p>
    </template>

    <!-- #actions is the given slot name from BaseDialog -->
    <template #actions>
      <base-button @click="confirmError">Okay</base-button>
    </template>
  </base-dialog>

  <!-- here comes the input form -->
  <base-card>
    <form @submit.prevent="submitData">
      <div class="form-control">
        <label for="title">Title</label>
        <input id="title" name="title" type="text" ref="titleInput" />
      </div>

      <div class="form-control">
        <label for="description">Description</label>
        <textarea
          id="description"
          name="description"
          rows="3"
          ref="descInput"
        ></textarea>
      </div>

      <div class="form-control">
        <label for="link">Link</label>
        <input id="link" name="link" type="url" ref="linkInput" />
      </div>

      <div class="form-control">
        <label for="pic">Picture url</label>
        <input id="pic" name="pic" type="url" ref="picInput" />
      </div>

      <div>
        <base-button type="submit">Add Recipe</base-button>
      </div>
    </form>
  </base-card>
</template>

<script>
import { ref, inject } from 'vue';

export default {
  setup() {
    let inputIsInvalid = ref(false); //at page load input is valid

    const titleInput = ref(null);
    const descInput = ref(null);
    const linkInput = ref(null);
    const picInput = ref(null); // const enteredTitle = ref(null); //instead of this.$refs.titleInput.value

    const addRecipe = inject('addRecipe'); //inject addRecipe from TheRecipes.vue methods

    function submitData() {
      const enteredTitle = titleInput.value.value;
      const enteredDescription = descInput.value.value;
      const enteredUrl = linkInput.value.value;
      const enteredPicUrl = picInput.value.value;
      //the 2nd .value is getting access to that value property of the input element object
      //The 1st .value gets access to the value of our ref, which then is that input element object
      //With that, we get access to the user input and store it in a const

      if (
        enteredTitle.trim() === '' ||
        enteredDescription.trim() === '' ||
        enteredUrl.trim() === '' ||
        enteredPicUrl.trim() === ''
      ) {
        inputIsInvalid.value = true;
        //if entered value is empty, we change inputIsInvalid to true, display the error dialog and return from this function
        //The trim() method removes whitespace from both ends of a string and returns a new string, without modifying the original string.
        return;
      }

      //else we send the input value as parameters to the addRecipe method
      addRecipe(enteredTitle, enteredDescription, enteredUrl, enteredPicUrl);
    }

    function confirmError() {
      inputIsInvalid.value = false; //restores inputIsInvalid back to false, if entered value is valid
    }

    return {
      inputIsInvalid,
      submitData,
      confirmError,
      titleInput,
      descInput,
      linkInput,
      picInput,
    };
  },

  /******** THE OPTIONS API ********/

  // inject: ['addRecipe'],

  // data() {
  //   return {
  //     inputIsInvalid: false,
  //   };
  // },

  // methods: {
  //   submitData() {
  //      //with $refs we take the value of the above input ref, just like querySelector().value and store it in a const
  //     const enteredTitle = this.$refs.titleInput.value;
  //     const enteredDescription = this.$refs.descInput.value;
  //     const enteredUrl = this.$refs.linkInput.value;
  //     const enteredPicUrl = this.$refs.picInput.value;

  //     if (
  //       enteredTitle.trim() === '' ||
  //       enteredDescription.trim() === '' ||
  //       enteredUrl.trim() === '' ||
  //       enteredPicUrl.trim() === ''
  //     ) {
  //       this.inputIsInvalid = true;
  //       return;
  //     }

  //     this.addRecipe(
  //       enteredTitle,
  //       enteredDescription,
  //       enteredUrl,
  //       enteredPicUrl
  //     );
  //   },

  //   confirmError() {
  //     this.inputIsInvalid = false;
  //   },
  // },
};
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>
