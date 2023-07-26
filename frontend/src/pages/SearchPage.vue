<template>
  <div class="main_section">
    <div class="search_section">
      <h3 style="color: rgba(0, 0, 0, 0.60);">Filters</h3>
      <span>Recipe name:</span>
      <b-form-input class="mb-3" v-model="searchQuery" size="sm"></b-form-input>
      <span>Cuisines:</span><vue-select class="mb-3" v-model="selectedCuisine" :options="cuisine_array"
        multiple></vue-select>
      <span>Diets:</span><vue-select class="mb-3" v-model="selectedDiet" :options="dietary_list" multiple></vue-select>
      <span>Intolerances:</span><vue-select class="mb-3" v-model="selectedIntolerances" :options="allergens_list"
        multiple></vue-select>
      <div>
        <b-button @click="searchRecipes" size="sm" class="my-2 my-sm-3 w-75 " variant="warning">Search</b-button>
        <b-form-select size="sm" class="w-25" v-model="numResults">
          <b-form-select-option value="5">5</b-form-select-option>
          <b-form-select-option value="10">10</b-form-select-option>
          <b-form-select-option value="15">15</b-form-select-option>
        </b-form-select>
      </div>
    </div>
    <div class="results_wrapper">
      <h2 id="title" class="text-center mt-5 mb-2">Search</h2>
      <div class="sorting_buttons" v-if="showResults">
        <b-button @click="toggleSort('popularity')" size="sm" class="my-2 my-sm-3 mr-2" variant="light">
          Sort by Popularity
          <i v-if="currentSortBy === 'popularity'" :class="{
            'bi-arrow-up': sortOrderPopularity,
            'bi-arrow-down': !sortOrderPopularity,
          }"></i>
        </b-button>
        <b-button @click="toggleSort('readyInMinutes')" size="sm" class="my-2 my-sm-3 ml-2" variant="light">
          Sort by Preparation Time
          <i v-if="currentSortBy === 'readyInMinutes'" :class="{
            'bi-arrow-up': sortOrderPreparation,
            'bi-arrow-down': !sortOrderPreparation,
          }"></i>
        </b-button>
      </div>
      <div class="last_results_recipes pl-2 ml-3" v-if="showResults">
            <div class="last_result_wrapper" v-for="r in recipes" :key="r.id">
            <RecipePreview class="recipePreview" :recipe="r" />
            </div>          
        </div>
      <div v-if="showNoResultsMsg" class="no_results_section text-center mt-5">
        <h3>No recipes found...</h3>
      </div>

      <div v-if="showLastWatched()" class="last_search_section">
        <div class="">
          <h3 style="color: rgba(0, 0, 0, 0.60);">Last Search</h3>
          <span v-if="last_search_params.query" class="mr-2 ml-1"><b>Recipe name:</b> {{ last_search_params.query }}
            |</span>
          <span v-if="last_search_params.cuisine" class="mr-2 ml-1"><b>Cuisine:</b> {{ last_search_params.cuisine }}
            |</span>
          <span v-if="last_search_params.diet" class="mr-2 ml-1"><b>Diet:</b> {{ last_search_params.diet }} |</span>
          <span v-if="last_search_params.intolerances" class="mr-2 ml-1"><b for="intolerances">Intolerances:</b>
            {{ last_search_params.intolerances }} |</span>
          <b-button @click="editLastSearch" size="sm" class="my-2 my-sm-3" variant="secondary">Edit Search</b-button>
        </div>
          <div class="last_results_recipes" >
            <div class="last_result_wrapper" v-for="r in last_search_results" :key="r.id">
            <RecipePreview class="recipePreview" :recipe="r" />
            </div>
          
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import RecipePreview from "../components/RecipePreview.vue";
import VueSelect from "vue-select";
import "vue-select/dist/vue-select.css";

export default {
  data() {
    return {
      showResults: false,
      showNoResultsMsg: false,
      recipes: [],
      last_search_params: this.$root.store.last_search_params,
      last_search_results: this.$root.store.last_search_results,
      currentSortBy: "",
      sortOrderPopularity: false, //True - ascending, False - descending
      sortOrderPreparation: true, //True - ascending, False - descending
      searchQuery: "",
      numResults: "5",
      selectedCuisine: [],
      selectedDiet: [],
      selectedIntolerances: [],
      cuisine_array: [
        "African",
        "Asian",
        "American",
        "British",
        "Cajun",
        "Caribbean",
        "Chinese",
        "Eastern European",
        "European",
        "French",
        "German",
        "Greek",
        "Indian",
        "Irish",
        "Italian",
        "Japanese",
        "Jewish",
        "Korean",
        "Latin American",
        "Mediterranean",
        "Mexican",
        "Middle Eastern",
        "Nordic",
        "Southern",
        "Spanish",
        "Thai",
        "Vietnamese",
      ],
      dietary_list: [
        "Gluten Free",
        "Ketogenic",
        "Vegetarian",
        "Lacto-Vegetarian",
        "Ovo-Vegetarian",
        "Vegan",
        "Pescetarian",
        "Paleo",
        "Primal",
        "Modified Paleo (Dairy allowed)",
        "Low FODMAP",
        "Whole30",
      ],
      allergens_list: [
        "Dairy",
        "Egg",
        "Gluten",
        "Grain",
        "Peanut",
        "Seafood",
        "Sesame",
        "Shellfish",
        "Soy",
        "Sulfite",
        "Tree Nut",
        "Wheat",
      ],
    };
  },
  methods: {
    async searchRecipes() {
      try {
        const params = {
          query: this.searchQuery,
          // TODO: comment before testing
          number: this.numResults,
          // number: 1,
          cuisine: this.selectedCuisine.join(","),
          diet: this.selectedDiet.join(","),
          intolerances: this.selectedIntolerances.join(","),
        };
        this.$root.store.last_search_params = params;
        const response = await axios.get(
          `${this.$root.store.server_domain}/recipes/search`,
          { params }
        );
        this.recipes = response.data;
        this.$root.store.last_search_results = this.recipes;
        if (this.recipes.length === 0) {
          this.showNoResultsMsg = true;
          this.showResults = false;
        } else {
          this.showNoResultsMsg = false;
          this.showResults = true;
        }
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },

    editLastSearch() {
      const last_search = this.last_search_params;
      this.searchQuery = last_search.query;
      this.numResults = last_search.number;
      this.selectedCuisine = last_search.cuisine.split(",");
      this.selectedDiet = last_search.diet.split(",");
      this.selectedIntolerances = last_search.intolerances.split(",");
    },
    showLastWatched() {
      if (this.last_search_params && !this.showResults) {
        // if(this.last_search_params){
        return true;
      } else {
        return false;
      }
    },
    toggleSort(sortBy) {
      if (this.currentSortBy === sortBy) {
        if (sortBy === "popularity") {
          this.sortOrderPopularity = !this.sortOrderPopularity; // Toggle sort order for popularity
        } else if (sortBy === "readyInMinutes") {
          this.sortOrderPreparation = !this.sortOrderPreparation; // Toggle sort order for readyInMinutes
        }
      } else {
        // Default sort order: popularity= descending, preparation= ascending
        this.currentSortBy = sortBy;
        this.sortOrderPopularity = sortBy === "popularity" ? false : true;
        this.sortOrderPreparation = sortBy === "readyInMinutes" ? true : false;
      }

      this.sortRecipes();
    },

    sortRecipes() {
      const { currentSortBy, sortOrderPopularity, sortOrderPreparation } = this;
      this.recipes.sort((a, b) => {
        const valueA = a[currentSortBy];
        const valueB = b[currentSortBy];

        if (currentSortBy === "popularity") {
          return sortOrderPopularity ? valueA - valueB : valueB - valueA;
        } else if (currentSortBy === "readyInMinutes") {
          return sortOrderPreparation ? valueA - valueB : valueB - valueA;
        }
        return 0;
      });
    },
    resultsNotEmpty() {
      return this.rec;
    },
  },
  components: {
    RecipePreview,
    VueSelect,
  },
};
</script>
<style>
.main_section {
  display: flex;
}

.results_section {
  width: 80%;
  display: flex;
  margin: auto;
  display: flex;
  overflow: scroll;
  overflow-y: hidden;
  justify-content: center;
  align-items: center;
}

.results_section::-webkit-scrollbar-track {
  background-color: #ffffff; 
    border-radius: 10px;
}

.results_section::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: #fac609; 
}

.results_section::-webkit-scrollbar {
  width: 8px;
}


.search_section {
  display: flex;
  flex-direction: column;
  background-color: rgba(237, 240, 237, 0.797);
  width: 350px;
  min-height: 100vh;
  padding: 20px;
  padding-top: 40px;

}
.results_wrapper {
  width: 85%;
  display: flex;
  flex-direction: column;

}

.sorting_buttons {
  margin: auto;
  margin-top: 0;
  margin-bottom: 10px;
}

#title {
  color: rgb(91, 184, 91);
  font-size: 35px;
}

.last_search_section {
  background-color: rgba(0, 0, 0, 0.048);
  margin-top: auto;
  padding: 20px;
  border: 3px dashed #00000037;
  border-radius: 20px;
  margin-left: 20px;
  margin-right: 20px;
  margin-bottom: 20px;
  width: 90%;
  margin: auto;
}

.last_results_recipes{
  display: flex;
  flex-direction: row;
  width: 98%;
  overflow: scroll;
  overflow-y: hidden;
}


.last_result_wrapper{
  margin-left: 20px;
  margin-right: 20px;
  border-radius: 20px;
  padding:8px;
}

.recipePreview{
  box-shadow: 0px 0px 10px #00000047;
  border-radius: 20px;
  width:auto;
}

.last_results_recipes::-webkit-scrollbar-track {
  background-color: #ffffff; 
    border-radius: 10px;
}

.last_results_recipes::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: #fac609; 
}

.last_results_recipes::-webkit-scrollbar {
  width: 8px;
}

</style>
