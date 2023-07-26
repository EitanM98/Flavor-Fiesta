
<template>
  <div class="container ">
    <div class="row">
      <div class="col">
        <h2 id="title" class="text-center mt-5">About Us</h2>
        <div class="w-75 mx-auto">
          <p class="text-center mt-4">
            Welcome to Flavor Fiesta, your ultimate destination for delicious
            food recipes! Whether you're a seasoned chef or a beginner in the
            kitchen, we have a wide range of recipes to satisfy your taste buds.
            From mouthwatering appetizers to delectable desserts, our collection
            will inspire you to create culinary masterpieces in your own home.
            Join us on this flavor-filled journey and let your taste buds dance
            with joy!
          </p>
        </div>
      </div>
    </div>

    <div class="row mt-5">
      <div class="col">
        <h2 id="title" class="text-center">Our Team</h2>
      </div>
    </div>

    <div id="team_section" class="row mt-3 ">
      <div class="col-md-6 d-flex align-items-center justify-content-center ">
        <div class="card">
          <img
            :src="ariel"
            id="team_image"
            class="card-img-top mx-auto"
            alt="Ariel Dawidowicz"
          />
          <div class="card-body">
            <h5 class="card-title text-center font-weight-light mb-4 mt-2">
              Ariel Dawidowicz
            </h5>
            <p class="card-text">
              Ariel is a passionate food enthusiast.
              With years of experience in the culinary
              world, Ariel brings a unique blend of flavors to our recipes. From
              traditional family recipes to innovative creations.
            </p>
            <h6 class="mt-5" style="text-decoration: underline;">Last Project</h6>
            <b>Personal Site - Andras Arato</b>
            <p class="project-description">
              A personal site featuring Andras Arato, also known as "Hide the Pain Harold," a famous meme.
              This project, created by Ariel, showcases Andras Arato's story.

            </p>
            <b-button href="https://web-development-environments-2023.github.io/324474857" size="sm" class=" w-25" variant="success">View Project</b-button>
          </div>
        </div>
      </div>

      <div class="col-md-6 d-flex align-items-center justify-content-center">
        <div class="card">
          <img
            :src="eithan"
            id="team_image"
            class="card-img-top  mx-auto"
            alt="Eithan Markman"
          />
          <div class="card-body">
            <h5 class="card-title text-center font-weight-light mb-4 mt-2">
              Eithan Markman
            </h5>
            <p class="card-text">
              Eithan is a culinary prodigy with a knack for experimentation. His
              inventive recipes push the boundaries of traditional cooking and
              offer a fresh perspective on flavor combinations. Eithan's
              creativity and attention to detail make him an invaluable member
              of our team.
            </p>
            <h6 class="mt-5" style="text-decoration: underline;">Last Project</h6>
            <b>Personal Site - Kareem Abdul Jabbar</b>
            <p class="project-description">
              A personal site about NBA legend Kareem Abdul Jabbar. Project
              created by Eithan, mainly focusing on basic HTML & CSS.
            </p>
            <b-button href="https://web-development-environments-2023.github.io/208649913/" size="sm" class=" w-25" variant="success">View Project</b-button>
          </div>
        </div>
      </div>
    </div>
<div class="together_projects mt-3 text-center mb-5 pb-3">
  <div>
    <b>Spaceships Attack Game</b>
  </div>
  <div>
  </div>
  <div>
    <p class="mt-3">
      "Spaceships Attack Game" is an exciting web game developed by our team, focused on HTML Canvas drawing.
       Take control of a spaceship and battle against enemy ships, inspired by games like "Chicken Invaders".
       Survive, shoot down enemies, and avoid their attacks. Progress through challenging levels, improve your skills, and achieve high scores.
      The game features a login system for saving progress and competing on the leaderboard. Enjoy engaging gameplay, immersive visuals,
       and thrilling space battles for players of all ages.

    </p>
    <b-button href="https://web-development-environments-2023.github.io/assignment2-208649913_324474857/#" size="sm" style="width: 150px;" variant="success">View Project</b-button>

  </div>
</div>
  </div>
  
</template>

<script>
export default {
  name: "AboutPage",
  data() {
    return {
      ariel: require("@/assets/ariel.png"),
      eithan: require("@/assets/eithan.png"),
    };
  },
};
</script>

<style scoped>

.container{
  display: flex;
  flex-direction: column;
  justify-content: center;
}
#team_section{
  width: 80%;
  margin: auto;
  height: 100%;
}

#team_image {
  width: 230px;
  border-radius: 100%;
  border: 7px solid #ffc107;
  box-shadow: 0 0 25px 1px #00000063;
}

.card {
  background-color: rgb(253, 253, 253);
  border-radius: 30px;
  padding-top: 20px;
  box-shadow: 0px 0px 15px #0000001d;
  height: 100%;
  border: 4px dashed #0000001b;

}

#title {
  color: rgb(91, 184, 91);
  font-size: 35px;
}

.together_projects{
  background-color: rgb(253, 253, 253);
  border-radius: 30px;
  box-shadow: 0px 0px 15px #0000001d;
  padding-top: 20px;
  padding: 15px;
  width: 80%;
  margin: auto;
  border: 4px dashed #0000001b;


}
</style>
