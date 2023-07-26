<template>
  <b-container class="container">
    <b-row>
      <div class="recipes_container" v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" :myRecipe="myRecipe" />
      </div>
    </b-row>
    <div v-if="isRecipesListEmpty">
      {{ this.not_found_message }}
    </div>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    },
    server_url: {
      type: String,
      required: true
    },
    myRecipe: {
      type: String,
      required: false
    },
    notFound_msg: {
      type: String,
      required: false,
      default: "No Recipes Found",
    }
  },
  data() {
    return {
      recipes: [],
      not_found_message: this.notFound_msg,
    };
  },
  mounted() {
    this.updateRecipes();
    
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + this.server_url,
          { withCredentials: true }
        );

        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(recipes)
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  },
  computed:{
    isRecipesListEmpty(){
      return this.recipes.length === 0;
    }
  }
};
</script>

<style scoped>


.container{
  display: flex;
  justify-content: center;
}
.recipes-row {
  flex-wrap: wrap;
  justify-content: center;
  height: 100%;
}

.recipePreview {
  margin: 20px;
}
</style>
