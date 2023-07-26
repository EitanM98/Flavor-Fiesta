<template>
    <div>
        <i v-if="favorited ||  marked"  class="bi bi-star-fill" style="font-size:20px"></i>
        <i v-if="!favorited && !marked " @click="markAsFavorite(recipeId)" class="bi bi-star" style="font-size:20px; cursor: pointer;"></i>
</div>
</template>

<script>

export default {
    data(){
        return{
            marked:false
        }
    },
    methods:{
    async markAsFavorite(recipeId){
      try{
        // Send request only if the user is logged in
        if (this.$root.store.username){
          const response = await this.axios.post( this.$root.store.server_domain + '/users/favorites', {recipeId:recipeId},
          { withCredentials: true }
          );
          this.marked = true;
        }
      }
      catch(error){
        console.log(error);
      }
      
    }
  },
  props:{
    recipeId:{
        type: Number,
        required:true
    },
    favorited:{
        type:Boolean,
        required:true
    }
  }

}
</script>

<style>

</style>