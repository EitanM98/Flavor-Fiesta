<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="light">
       <b-navbar-brand><img class="logo" :src="logo" alt="logo Icon" width="60" /></b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item><router-link class="nav_link" :to="{ name: 'main' }">Main</router-link></b-nav-item>
          <b-nav-item><router-link class="nav_link" :to="{ name: 'search' }">Search</router-link></b-nav-item>
          <b-nav-item><router-link class="nav_link" :to="{ name: 'about' }">About</router-link></b-nav-item>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown v-if="$root.store.username" right>
            <!-- Using 'button-content' slot -->
            <template #button-content >
              <em style="color:rgba(91, 184, 91, 0.704);">{{ $root.store.username }}<i class="bi bi-caret-down-fill ml-2" ></i></em>
            </template>
            <b-dropdown-item ><router-link class="nav_link" :to="{ name: 'myFavoriteRecipes' }">My favorite
                recipes</router-link></b-dropdown-item>
            <b-dropdown-item><router-link class="nav_link" :to="{ name: 'myRecipes' }">My
                recipes</router-link></b-dropdown-item>
            <b-dropdown-item><router-link class="nav_link" :to="{ name: 'myFamilyRecipes' }">My family
                recipes</router-link></b-dropdown-item>
          </b-nav-item-dropdown>
          <b-button v-if="$root.store.username" variant="success" size="sm" class="my-2 my-sm-1 ml-2 mr-2" @click="logout">Logout</b-button>
          <b-nav-item v-if="!$root.store.username">
            <span v-if="!$root.store.username" style="color:rgba(91, 184, 91, 0.704);">Hello guest</span>
            <b-button size="sm" class="my-2 my-sm-0 ml-3" variant="success">
              <router-link class="nav_btn" :to="{ name: 'login' }">Login</router-link>
            </b-button>
            <b-button size="sm" class="my-2 my-sm-0 ml-3" variant="success">
              <router-link class="nav_btn" :to="{ name: 'register' }">Register</router-link>
            </b-button>
          </b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <router-view class="router" :style="{ backgroundImage: `url(${bg})` }" />
    <div class="footer">
      All rights reserved to Ariel Dawidowicz & Eithan Markman ©
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      selectedOption: "select",
      logo:require('@/assets/logo.png'),
      bg:require('@/assets/bg.jpg'),

    };
  },
  methods: {
    
    logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
  },
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

.nav_link {

  color: rgb(91, 184, 91);
  transition: ease-in-out .1s;
}

.nav_btn{
  color: white;
}

.nav_btn:hover{
  color: white;
  text-decoration: none;
}


.nav_link:hover{
  color: rgb(50, 108, 50);
  text-decoration: none;
}
.footer{
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(213, 202, 152, 0.705);
}

.router{
  min-height: 100vh;
  min-width: 100vw;
  background-size: auto;
  background-position: center;
}

</style>
