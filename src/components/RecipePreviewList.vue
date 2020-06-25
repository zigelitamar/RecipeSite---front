<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-col>
      <b-row v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-row>
    </b-col>
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
        }
        const response = await this.axios.get(endpoint, {
          withCredentials: true
        });
        const recipes = response.data;
        if (this.$root.store.username) {
          let ids = [];
          recipes.forEach(element => {
            ids.push(element.id);
          });

          const res = await this.axios.get(
            "https://recipestest1.herokuapp.com/user/recipeInfo/" +
              JSON.stringify(ids),
            {
              withCredentials: true
            }
          );

          for (let key in res.data) {
            recipes.forEach(recipe => {
              if (recipe.id == key) {
                recipe.watched = res.data[key].watched;
                recipe.favorite = res.data[key].favorite;
              }
            });
          }
        }
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
