<template>
    <div class="container">
      <h1 id="title" class="text-center pt-5 mb-3">Favorites Page</h1>
      <RecipePreviewList  title="Favorite Recipes " server_url="/users/favorites" 
      notFound_msg="You have not added any favorite recipes yet" class="favorite_list" />
    </div>
</template>
  
  <script>
  import RecipePreviewList from "../components/RecipePreviewList";
  export default {
    components: {
      RecipePreviewList
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
.container{
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.favorite_list{
  margin: auto;
}
  </style>
  