<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.recipe_name }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in: {{ recipe.duration }}</div>

            </div>Ingredients:
            <ul>
              <li
                v-for=" r in recipe.ingredients"
                :key="r.id"
              >{{ r.name }} - {{r.quantity}} {{r.unit}}</li>
            </ul>
          </div>
          <div class="wrapped">
            Instructions:
            <ol>
              <li v-for="s in recipe._instructions" :key="s">{{ s }}</li>
            </ol>
          </div>
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      >-->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null
    };
  },
  async created() {
    try {
      let response;

      try {
        const id = this.$route.params.recipeId;
        let url = "https://recipestest1.herokuapp.com/user/fullPersonalRecipe/" + id;
        response = await this.axios.get(url);

        if (response.status !== 200) this.$router.replace("/NotFound");
      
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }

      let {
        recipe_name,
        instructions,
        ingredients,
        duration,
        image,
      } = response.data;

      let _instructions = instructions.split("\n")
      console.log(_instructions)

      let _recipe = {
         recipe_name,
        _instructions,
        ingredients,
        duration,
        image,
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
