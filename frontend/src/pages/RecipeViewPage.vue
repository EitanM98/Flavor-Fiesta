<template>
  <div class="container">
    <div class="containter_info" v-if="recipe">
      <div class="recipe-header mt-3 mb-4 d-flex">
        <div class="image_wrapper center" :style="`background-image: url(${recipe.image})`"></div>
        <div class="header_info">
          <h1 class="text-center mb-3">{{ recipe.title }}</h1>
          <div class="d-flex cente justify-content-center">
            <i
              v-if="$root.store.username && recipe.watched"
              class="bi bi-eye-fill ml-1 mr-1"
              style="font-size:20px"
            ></i>
            <FavoriteStar
              class="ml-1 mr-1"
              v-if="$root.store.username && recipe.watched !== undefined"
              :favorited="recipe.favorite"
              :recipeId="recipe.id"
            ></FavoriteStar>
            <img
              class="ml-1 mr-1"
              v-if="recipe.vegan"
              :src="vegan"
              alt="Vegan Icon"
              width="50px"
              height="30px"
            />
            <img
              class="ml-1 mr-1"
              v-if="recipe.vegetarian"
              :src="vegetarian"
              alt="Vegan Icon"
              width="35px"
              height="35px"
            />
            <img
              class="ml-1 mr-1"
              v-if="recipe.glutenFree"
              :src="glutenFree"
              alt="glutenFree Icon"
              width="30px"
              height="30px"
            />
          </div>

          <span class="mt-2"
            ><i class="bi bi-clock mr-2"></i> <b>Ready in:</b>
            {{ recipe.readyInMinutes }} minutes</span
          >
          <span class="mt-2" v-if="this.$route.params.myRecipe !== 'true'"
            ><i class="bi bi-hand-thumbs-up mr-2"></i> <b>Likes:</b>
            {{ recipe.popularity }} likes
          </span>
          <span class="mt-2"
            ><i class="bi bi-person mr-2"></i><b>Serving:</b>
            {{ recipe.servings }}</span
          >
        </div>
      </div>

      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3"></div>
            <b>Ingredients:</b>
            <ul v-if="this.$route.params.myRecipe !== 'true'">
              <li
                v-for="(r, index) in recipe.extendedIngredients"
                :key="index + '_' + r.id"
              >
                {{ r.original }}
              </li>
            </ul>
            <div v-if="this.$route.params.myRecipe === 'true'" v-html="recipe.ingredients"></div>
          </div>
          <div class="wrapped">
            <b>Instructions</b>
            <div v-html="recipe.instructions"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import FavoriteStar from "../components/FavoriteStar.vue";
export default {
  data() {
    return {
      recipe: null,
      vegan: require("@/assets/vegan_icon.png"),
      vegetarian: require("@/assets/vegetarian_icon.png"),
      glutenFree: require("@/assets/gluten_free_icon.png"),
    };
  },
  async created() {
    try {
      let response;
      try {
        if (this.$route.params.myRecipe === undefined) {
          response = await this.axios.get(
            `${this.$root.store.server_domain}/recipes/${this.$route.params.recipeId}`,
            { withCredentials: true }
          );
          console.log(response);

          if (response.status !== 200) this.$router.replace("/NotFound");
        }
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      let isMyRecipe = this.$route.params.myRecipe;
      let {
        instructions,
        extendedIngredients,
        popularity,
        readyInMinutes,
        image,
        title,
        vegan,
        vegetarian,
        glutenFree,
        servings,
        ingredients,
        favorite,
      } = isMyRecipe === "true" ? this.$route.params.recipe : response.data;

      let _recipe = {
        instructions,
        extendedIngredients,
        popularity,
        readyInMinutes,
        image,
        title,
        vegan,
        vegetarian,
        glutenFree,
        servings,
        ingredients,
        favorite,
      };

      this.recipe = _recipe;
      if (isMyRecipe === undefined)
        this.markAsWatched(this.$route.params.recipeId);
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    async markAsWatched(recipeId) {
      try {
        if (this.$root.store.username) {
          const response = await this.axios.post(
            this.$root.store.server_domain + "/recipes/watched",
            { recipe_id: recipeId },
            { withCredentials: true }
          );
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
  components: {
    FavoriteStar,
  },
};
</script>

<style scoped>
.wrapper {
  display: flex;
}

.wrapped {
  width: 50%;
}

.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}

.container {
  padding-top: 30px;
  padding-bottom: 30px;
}

.containter_info {
  background-color: rgb(245, 239, 215);
  width: 90%;
  max-width: 1200px;
  margin: auto;
  padding: 20px;
  box-shadow: 0px 0px 15px #0000002e;
  border-radius: 30px;
}

.header_info {
  display: flex;
  flex-direction: column;
  background-color: #a1b65628;
  width: 80%;
  padding: 20px;
  margin-left: 20px;
  border-radius: 20px;
}

.recipe-body {
  background-color: #a1b65628;
  padding: 20px;
  border-radius: 20px;
}

.header_info h1 {
  font-size: 25px;
}

.recipe_img {
}
.image_wrapper{
  width: 40%;
  height: 300px;
  border-radius: 20px;
  background-size: cover;
  background-position: center;
  transition: .2s ease-in-out;
  border: 5px solid #a1b65658;
}

</style>
