<template>
  <div class="container">
    <h1 class="text-center pt-5 pb-5" id="title">Family Recipes Page</h1>
    <FamilyRecipeList title="Family Recipes" server_url="/users/family-recipes" 
    notFound_msg="This user has not added family recipes" class="RandomRecipes center" />
  </div>
</template>

<script>
import FamilyRecipeList from "../components/FamilyRecipeList";
export default {
  components: {
    FamilyRecipeList
  }
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

#title {
  color: rgb(91, 184, 91);
  font-size: 35px;
}
</style>
