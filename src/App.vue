<script>
import { ref } from "vue";
import Footer from "@/components/Footer.vue";
import Loading from "@/components/Loading.vue";
import Header from "@/components/Header.vue";
import Chatbot from "@/components/Chatbot.vue"; // Import the Chatbot component
import recipes from "./recipes.js";
import RecipesList from "@/components/RecipesList.vue";

export default {
  components: { Footer, Loading, Header, Chatbot, RecipesList },
  setup() {
    const loading = ref(false);

    function updateLoadingStatus(status) {
      loading.value = status;
    }

    return { loading, updateLoadingStatus };
  },

  // Recipes data
  data() {
    return {
      recipes: recipes,
    };
  },

  // CRUD methods
  methods: {
    addRecipe(recipe) {
      // ...
    },
    deleteRecipe(id) {
      // ...
    },
    updateRecipe(recipe) {
      // ...
    },
  },
};
</script>

<template>
  <div>
    <Header />
    <Chatbot />
    <RecipesList
      :recipes="recipes"
      :addRecipe="addRecipe"
      :deleteRecipe="deleteRecipe"
    />
    <Loading v-if="loading" />
    <div v-show="loading === false">
      <router-view @loadingStatus="updateLoadingStatus" />
      <Footer />
    </div>
  </div>
</template>
