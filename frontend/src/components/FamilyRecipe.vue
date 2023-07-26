<template>
    <div class="container d-flex mb-5">
      <div class="container_info" v-if="recipe">
        <div class="header_info mt-3 mb-4">
          <img v-if="isValidImagePath" :src="getImage()" class="recipe_img" />
          <div class="header_data">
                <h1 class="text-center">{{ recipe.title }}</h1>
                <span class="mt-2"><i class="bi bi-clock mr-2"></i> <b>Ready in: </b> {{ recipe.readyInMinutes }} minutes</span>
                <span class="mt-2"><i class="bi bi-c-circle mr-2"></i><b>Invented by: </b> {{ recipe.creator }} </span>
                <span class="mt-2"><i class="bi bi-calendar-check mr-2"></i><b>Typically prepared on: </b> {{ recipe.when_to_prepare }} </span>
            </div>
        </div>
        <div class="recipe-body">
          <div class="wrapper">
            <div class="wrapped">
              
              <b>Ingredients:</b>
              <ul>
                <li
                  v-for="(ingredient, index) in parsedIngredients" :key="index">
                  {{ ingredient }}
                </li>
              </ul>
            </div>
            <div class="wrapped">
              <b>Instructions</b>
              <ol>
                <li
                  v-for="(instruction, index) in parsedInstructions" :key="index">
                  {{ instruction }}
                </li>
              </ol>
            </div>

          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>

    export default {
    props: {
        recipe: {
        type: Object,
        required: true,
        },
    },
    data(){
        return{
            image:null
        }
    },
    computed: {
        parsedIngredients(){
            return this.recipe.ingredients.split(";");
        },
        parsedInstructions(){
            return this.recipe.instructions.split(";");
        },
        isValidImagePath() {
            return this.getImage() !== null;
        },
    
    },
    methods: {
        getImage() {
        try {
            return require('@/assets/family_recipes_images/' + this.recipe.image)
        } catch (error) {
            return null;
        }
        },
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
  .header_data{
    display: flex;
  flex-direction: column;
  background-color: #a1b65628;
  width: 80%;
  padding: 20px;
  margin-left: 20px;
  border-radius: 20px;
  }

  .header_data h1{
    font-size: 25px;
  }

  .header_info{
    display: flex;
    flex-direction: row;
  }

  .container_info {
  background-color: rgb(245, 239, 215);
  width: 100%;
  max-width: 1200px;
  margin: auto;
  padding: 20px;
  box-shadow: 0px 0px 15px #0000002e;
  border-radius: 30px;
}

.recipe_img{
  border: 5px solid #a1b65658;
  border-radius: 20px;
  width: 40%;
  height: 300px;
}

.recipe-body{
  background-color: #a1b65628;
  padding: 20px;
  border-radius: 20px;
}
  </style>
  