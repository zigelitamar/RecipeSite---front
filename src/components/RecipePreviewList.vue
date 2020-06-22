<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" :rType="rType" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    },
    rType: {
      type: String,
      required: false,
      default: "MyRecipe"
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    console.log(this.$cookies.get("session"));
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const type = this.rType;
        let endpoint = "";
        if (type == "rand") {
          endpoint = "https://recipestest1.herokuapp.com/recipes/randomrecipe";
        } else if (type == "lastSeen" && this.$root.store.username) {
          endpoint = "https://recipestest1.herokuapp.com/user/lastseen";
        } else {
          endpoint = "https://recipestest1.herokuapp.com/recipes/randomrecipe";
        }
        console.log(type);
        console.log(endpoint);
        const response = await this.axios.get(endpoint, {
          withCredentials: true
        });

        console.log(response);

        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
