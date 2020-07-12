<template>
  <div>
    <h2>serch results for :</h2>
    <div class="grid">
      <div v-for="r in recipes" :key="r.id">
        <RecipePreview :recipe="r" />
      </div>
    </div>
  </div>
</template>

<script>
import RecipePreview from "./RecipePreview";
export default {
  name: "GridSearchRecipe",
  components: {
    RecipePreview
  },
  props: {
    query: {
      type: string,
      requierd: true
    },
    amount: {
      type: integer,
      requierd: true
    },
    diet: {
      type: string
    },
    cuisine: {
      type: string
    },
    intolerance: {
      type: string
    }
  },
  data() {
    return {
      recipes: []
    };
  },

  created() {
    this.searchAndOreder();
  },
  methods: {
    async searchAndOreder() {
      try {
        let search_params = {};
        if (diet != null) {
          search_params.diet = this.diet;
        }
        if (cuisine != null) {
          search_params.cuisine = this.cuisine;
        }
        if (intolerance != null) {
          search_params.intolerance = this.intolerance;
        }

        const res = await this.axios.get(
          `https://recipestest1.herokuapp.com/recipes/search/query/${this.query}/amount/${this.amount}`,
          { params: search_params }
        );
        let all = res.data;
        console.log(all);
        this.recipes = [];
        this.recipes.push(...all);
      } catch (err) {
        console.log(err.res);
        this.form.submitError = err.res.data.message;
      }
    }
  }
};
</script>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(80px, auto);
}
