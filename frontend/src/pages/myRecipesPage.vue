<template>
  <div class="contrainer">
    <h2 id="title" class="text-center pt-5 mb-5">My recipes</h2>
    <b-button class="new_button" v-b-modal.modal-prevent-closing @click="showModal" variant="success">+ New recipe</b-button>
    <div class="main_container">
      <b-modal
        id="modal-prevent-closing"
        ref="modal"
        title="Submit Your Recipe"
        @show="resetModal"
        @hidden="resetModal"
        @ok="handleOk"
      >
        <form ref="form" @submit.stop.prevent="handleSubmit">
          <b-form-group
            label="Title"
            label-for="title-input"
            invalid-feedback="Title is required"
            :state="titleState"
          >
            <b-form-input
              id="title-input"
              v-model="title"
              :state="titleState"
              required
            ></b-form-input>
          </b-form-group>
          <b-form-group
            label="Image"
            label-for="image-input"
            invalid-feedback="Image is required"
            :state="imageState"
          >
            <b-form-input
              id="image-input"
              v-model="image"
              :state="imageState"
              required
            ></b-form-input>
          </b-form-group>
          <b-form-group
            label="Ready in minutes"
            label-for="readyInMinutes-input"
            invalid-feedback="Ready in minutes is required"
            :state="readyInMinutesState"
          >
            <b-form-input
              id="readyInMinutes-input"
              v-model="readyInMinutes"
              :state="readyInMinutesState"
              required
            ></b-form-input>
          </b-form-group>
          <b-form-group
            label="Servings"
            label-for="servings-input"
            invalid-feedback="Servings is required"
            :state="servingsState"
          >
            <b-form-input
              id="servings-input"
              v-model="servings"
              :state="servingsState"
              required
            ></b-form-input>
          </b-form-group>
          <b-form-group
            label="Ingredients (separated by ;)"
            label-for="ingredients-input"
            invalid-feedback="ingredients is required"
            :state="ingredientsState"
          >
            <b-form-input
              id="ingredients-input"
              v-model="ingredients"
              :state="ingredientsState"
              required
            ></b-form-input>
          </b-form-group>
          <b-form-group
            label="Instructions (separated by ;)"
            label-for="instructions-input"
            invalid-feedback="Instructions is required"
            :state="instructionsState"
          >
            <b-form-input
              id="instructions-input"
              v-model="instructions"
              :state="instructionsState"
              required
            ></b-form-input>
          </b-form-group>
          <div class="checkbox_gorup">
            <b-form-checkbox id="vegetarian-input" v-model="vegetarian" required
              >Vegetarian</b-form-checkbox
            >
            <b-form-checkbox id="vegan-input" v-model="vegan" required
              >Vegan</b-form-checkbox
            >
            <b-form-checkbox id="glutenFree-input" v-model="glutenFree" required
              >Gluten free</b-form-checkbox
            >
          </div>
        </form>
      </b-modal>
      <div>
        <b-container class="container">
          <b-row>
            <div class="recipes_container" v-for="r in this.recipes" :key="r.id">
              <RecipePreview class="recipePreview" :recipe="r" :myRecipe="'true'" />
            </div>
          </b-row>
          <div v-if="isRecipesListEmpty">
            <span>No recipes added yet</span>
          </div>
        </b-container>
      </div>
    </div>
  </div>
</template>

<script>
import { BModal } from "bootstrap-vue";
import RecipePreviewList from "../components/RecipePreviewList.vue";
import RecipePreview from "../components/RecipePreview.vue";

export default {
  data() {
    return {
      recipes: [],
      render: 0,
      title: "",
      image: "",
      readyInMinutes: "",
      vegetarian: false,
      vegan: false,
      glutenFree: false,
      servings: "",
      ingredients: "",
      instructions: "",
      titleState: null,
      imageState: null,
      readyInMinutesState: null,
      servingsState: null,
      instructionsState: null,
      ingredientsState: null,
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/my-recipes",
          { withCredentials: true }
        );
        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
      } catch (error) {
        console.log(error);
      }
    },
    showModal() {
      this.$root.$emit("bv::show::modal", "modal-prevent-closing");
    },

    checkFormValidity() {
      const valid = this.$refs.form.checkValidity();
      this.titleState = valid;
      this.imageState = valid;
      this.readyInMinutesState = valid;
      this.servingsState = valid;
      this.ingredientsState = valid;
      this.instructionsState = valid;
      return valid;
    },
    resetModal() {
      this.title = "";
      this.image = "";
      this.readyInMinutes = "";
      this.vegetarian = false;
      this.vegan = false;
      this.glutenFree = false;
      this.servings = "";
      this.ingredients = "";
      this.instructions = "";
      this.titleState = null;
      this.imageState = null;
      this.readyInMinutesState = null;
      this.servingsState = null;
      this.ingredientsState = null;
      this.instructionsState = null;
    },
    async handleSubmit() {
      if (!this.checkFormValidity()) {
        return false;
      }
      try {
        this.parseMyRecipeParams();
        console.log(this.ingredients);
        console.log(this.instructions);
        const response = await this.axios.post(
          this.$root.store.server_domain + "/users/my-recipes",
          {
            title: this.title,
            image: this.image,
            readyInMinutes: this.readyInMinutes,
            vegetarian: this.vegetarian,
            vegan: this.vegan,
            glutenFree: this.glutenFree,
            servings: this.servings,
            ingredients: this.ingredients,
            instructions: this.instructions,
          },
          { withCredentials: true }
        );
      } catch (error) {
        console.log(error);
      }
      return true;
    },
    handleOk(bvModalEvent) {
      // Prevent modal from closing
      bvModalEvent.preventDefault();

      // Trigger submit handler
      const res = this.handleSubmit();
      if (!res) {
        return;
      }
      alert("good");
      // Wait for the modal to be hidden
      this.$nextTick(() => {
        // Hide the modal manually
        this.$refs.modal.hide();
      });
      // Refresh page
      location.reload();
    },
    parseMyRecipeParams(){
      let ings = this.ingredients.split(";").map(item => `<li>${item}</li>`);
      let instructs = this.instructions.split(";").map(item => `<li>${item}</li>`);
      ings = `<ul>${ings.join("")}</ul>`;
      instructs = `<ol>${instructs.join("")}</ol>`;
      this.ingredients = ings;
      this.instructions = instructs;
    },
  },
  components: {
    BModal,
    RecipePreview
  },
  computed:{
    isRecipesListEmpty(){
      return this.recipes.length === 0;
    }
  }
};
</script>

<style scoped>
.main_container {
  display: flex;
  justify-content: center;
  align-items: center;
}
.checkbox_gorup {
  display: flex;
  justify-content: space-around;
}

#title {
  color: rgb(91, 184, 91);
  font-size: 35px;
}

.contrainer{
  display: flex;
  flex-direction: column;
}

.new_button{
  width: 130px;
  margin: auto;
}
</style>
