<template>
  <b-container>
    <h3>
      My family recipes
      <slot></slot>
    </h3>
    <b-col>
      <b-row v-for="r in recipes" :key="r.id">
        <PersonalRecipe :recipe="r" />
      </b-row>
    </b-col>
    <div v-if="!haveFamRec">
      <b-jumbotron bg-variant="dark" text-variant="white" border-variant="light">
        <template v-slot:header>Hi there rookie</template>

        <template v-slot:lead>
          We in "Recipe Planet" hope you had an easy experience signing up!
          Unfortunately you have not yet added family recipes,
          We will add this option soon, in the meantime enjoy the collection of recipes offered on the site
          you can explore via the 'explore recipes'
          button on the bottom of the "
          <router-link :to="{ name: 'main' }">Home</router-link>" screen or use our excellent search system
        </template>

        <hr class="my-4" />

      </b-jumbotron>
    </div>
  </b-container>
</template>

<script>
import PersonalRecipe from "../components/PersonalRecipe";
export default {
  components: {
    PersonalRecipe
  },

  data() {
    return {
      recipes: [],
      haveFamRec: true,
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        let endpoint = "";
        endpoint = "https://recipestest1.herokuapp.com/user/getfamilyrecipes";
        const response = await this.axios.get(endpoint, {
          withCredentials: true
        });
        if(response.status==204){
          this.haveFamRec=false;
        }
        this.recipes = [];
        this.recipes.push(...response.data);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style>
</style>