<template>
  <div>
    <!-- Edit Recipe Form -->
    <div v-if="isEditing" class="edit-recipe-form">
      <input v-model="editedRecipe.title" placeholder="Recipe Title" />
      <textarea
        v-model="editedRecipe.ingredients"
        placeholder="Ingredients (comma-separated)"
      ></textarea>
      <button @click="updateRecipe">Update Recipe</button>
      <button @click="isEditing = false">Cancel</button>
    </div>

    <!-- Add Recipe Form -->
    <div class="add-recipe-form">
      <input v-model="newRecipeTitle" placeholder="Recipe Title" />
      <button @click="addRecipe">Add Recipe</button>
    </div>

    <transition-group name="list" tag="div" class="recipes">
      <RecipeItem
        v-for="recipe in localRecipes"
        :key="recipe.id"
        :recipe="recipe"
      />

      <button v-for="recipe in localRecipes" @click="deleteRecipe(recipe.id)">
        Delete {{ recipe.title }}
      </button>
      <button v-for="recipe in localRecipes" @click="editRecipe(recipe)">
        Edit {{ recipe.title }}
      </button>
    </transition-group>
  </div>
</template>

<script>
import RecipeItem from "./RecipeItem.vue";
import recipes from "../recipes.js";

export default {
  data() {
    return {
      newRecipeTitle: "", // For the new recipe input
      localRecipes: recipes, // Local copy of the recipes array
      editedRecipe: null, // For the recipe being edited
      isEditing: false, // Flag to show/hide the edit form
    };
  },

  methods: {
    addRecipe() {
      if (this.newRecipeTitle.trim() === "") return; // Check if input is not empty

      // Create a new recipe object
      const newRecipe = {
        id: Math.max(...this.localRecipes.map((r) => r.id)) + 1, // Generate a new ID
        title: this.newRecipeTitle,
        ingredients: [], // Empty ingredients for now
      };

      // Add the new recipe to the local recipes list
      this.localRecipes.push(newRecipe);

      // Clear the input
      this.newRecipeTitle = "";
    },

    deleteRecipe(id) {
      // Remove the recipe with the given ID from the local recipes list
      this.localRecipes = this.localRecipes.filter(
        (recipe) => recipe.id !== id
      );
    },

    editRecipe(recipe) {
      this.editedRecipe = { ...recipe }; // Create a copy of the recipe to edit
      this.isEditing = true; // Flag to show the edit form
    },

    updateRecipe() {
      const index = this.localRecipes.findIndex(
        (r) => r.id === this.editedRecipe.id
      );
      if (index !== -1) {
        this.localRecipes[index] = this.editedRecipe;
        this.isEditing = false; // Hide the edit form
      }
    },
  },
};
</script>

<style>
/* styles */

/* Add transitions for the recipes list */
.list-enter-active,
.list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active in <2.1.8 */ {
  opacity: 0;
  transform: translateY(-30px);
}
</style>
