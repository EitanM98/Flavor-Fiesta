<template>
  <div class="conteinter">
    <div class="center" v-if="recipes.length > 0 ">
      <b-row  v-for="r in recipes" :key="r.id">
        <FamilyRecipe class="" :recipe="r" />
      </b-row>
    </div>
    <p v-else><b> No family recipes found for this user.</b></p>
  </div>
</template>

<script>
import FamilyRecipe from "./FamilyRecipe.vue";
export default {
  name: "FamilyRecipeList",
  components: {
    FamilyRecipe
  },
  props: {
    title: {
      type: String,
      required: true
    },
    server_url: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
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

        // console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(recipes)
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}

.data{
  display: flex;
  flex-direction: row;
}
</style>
