<template>
  <b-container>
    <h3>
      Hi
      <slot></slot>
    </h3>
    <b-col>
      <b-row v-for="r in recipes" :key="r.id">
        <PersonalRecipe :recipe="r" />
      </b-row>
    </b-col>
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
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        let endpoint = "";
        endpoint = "https://recipestest1.herokuapp.com/user/getpersonalrecipes";
        const response = await this.axios.get(endpoint, {
          withCredentials: true
        });

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