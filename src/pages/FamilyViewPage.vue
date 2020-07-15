<template>
  <div>
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.recipe_name }}</h1>
        <img :src="recipe.image" class="center" height="330" width="200" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>author: {{ recipe.author }}</div>
              <div>we made this on: {{ recipe.occasions }}</div>
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
        <p v-for="s in recipe._instructions" :key="s">{{ s }}</p>
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
        let url =
          "https://recipestest1.herokuapp.com/user/fullFamilyRecipe/" + id;
        response = await this.axios.get(url);

        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }

      let {
        recipe_name,
        author,
        occasions,
        instructions,
        ingredients,
        duration,
        image
      } = response.data;

      let _instructions = instructions.split("\n");
      console.log(_instructions);

      let _recipe = {
        recipe_name,
        author,
        occasions,
        _instructions,
        ingredients,
        duration,
        image
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>

	.p{	
  font-family: 'Amaranth';font-size: 22px;	
  color:  #ffffff;	
  font-display: inherit;	
}
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
