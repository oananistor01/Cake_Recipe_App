<template>
  <!-- here comes the error message displayed when input is not entered right -->
  <base-dialog
    v-if="inputIsInvalid"
    title="Invalid Input"
    @close="confirmError"
  >
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
export default {
  //inject addRecipe from TheRecipes.vue methods
  inject: ['addRecipe'],

  data() {
    return {
      //at page load input is valid
      inputIsInvalid: false,
    };
  },

  methods: {
    submitData() {
      //with $refs we take the value of the above input ref, just like querySelector().value and store it in a const
      const enteredTitle = this.$refs.titleInput.value;
      const enteredDescription = this.$refs.descInput.value;
      const enteredUrl = this.$refs.linkInput.value;
      const enteredPicUrl = this.$refs.picInput.value;

      //if entered value is empty, we change inputIsInvalid to true
      if (
        enteredTitle.trim() === '' ||
        enteredDescription.trim() === '' ||
        enteredUrl.trim() === '' ||
        enteredPicUrl.trim() === ''
      ) {
        this.inputIsInvalid = true;
        return;
      }

      //else we send the input value as parameters to the addRecipe method
      this.addRecipe(
        enteredTitle,
        enteredDescription,
        enteredUrl,
        enteredPicUrl
      );
    },

    //restores inputIsInvalid back to false, if entered value is valid
    confirmError() {
      this.inputIsInvalid = false;
    },
  },
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
