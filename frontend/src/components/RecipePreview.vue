<template>

  <div>

        <b-card
        tag="article"
        style="max-width: 20rem;"
        class="mb-2"
        >
        <h2 class="text-center" style="font-size: 25px">{{ recipe.title }}</h2>
        <router-link
        :to="{ name: 'recipe', params: { recipeId: recipe.id, recipe:recipe, myRecipe:myRecipe }}"
        class="recipe-preview">
        <div class="image_wrapper center" :style="`background-image: url(${recipe.image})`"></div>
      </router-link>
      <b-card-text> <b>Ready in:</b> {{ recipe.readyInMinutes }} minutes </b-card-text>
      <b-card-text v-if="recipe.popularity>0"> <b>Popularity:</b> {{ recipe.popularity }} likes </b-card-text>
      <b-card-text v-if="recipe.servings>0"> <b>Servings:</b> {{ recipe.servings }} </b-card-text>

      <div class="data_wrapper">
      <div class="data">
        <i v-if="recipe.watched" class="bi bi-eye-fill ml-1 mr-1" style="font-size:20px" ></i>
        <FavoriteStar class="ml-1 mr-1" v-if="$root.store.username" :recipeId="recipe.id" :favorited="recipe.favorite"/>
        <img class="ml-1 mr-1" v-if="recipe.vegan" :src="vegan" alt="Vegan Icon" width="50px" height="30px" />
        <img class="ml-1 mr-1" v-if="recipe.vegetarian" :src="vegetarian" alt="Vegan Icon" width="35px" height="35px" />
        <img class="ml-1 mr-1" v-if="recipe.glutenFree" :src="glutenFree" alt="glutenFree Icon" width="30px" height="30px" />
      </div>
      <router-link
        :to="{ name: 'recipe', params: { recipeId: recipe.id, recipe:recipe, myRecipe:myRecipe }}"
        class="recipe-preview">
        <div class="buttonWrapper">
        <b-button  id="watchBtn" variant="warning">Watch full recipe</b-button>
      </div>
      </router-link>
    </div>
    </b-card>
  </div>
</template>

<script>
import FavoriteStar from './FavoriteStar.vue';

export default {
  data() {
    return {
      vegan: require('@/assets/vegan_icon.png'),
      vegetarian: require('@/assets/vegetarian_icon.png'),
      glutenFree: require('@/assets/gluten_free_icon.png'),
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true,
    },
    myRecipe: {
      type: String,
      required: false,
    },
  },
  components:{
    FavoriteStar
  }
};
</script>

<style scoped>
.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}

.buttonWrapper{
  width: 100%;
  display: flex;
}

#watchBtn{
  margin: auto;
}

.data{
  display: flex;
  justify-content: center;
  align-items: center;
}

.card{
  border-radius: 20px;
  height: 100%;
  display: flex;
  background-color: rgb(255, 255, 255);
}

.image_wrapper{
  width:260px;
  height: 200px;
  border-radius: 10px;
  background-size: cover;
  background-position: center;
  transition: .2s ease-in-out;
}

.image_wrapper:hover{
  transform: scale(1.05);
  box-shadow: 0px 0px 20px  #0000003d;

}
</style>
