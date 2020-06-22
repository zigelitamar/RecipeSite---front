<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <b-button @click="addfave">
          <img src="../images/pngguru.com.png" />
        </b-button>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in: {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.aggregateLikes }} likes</div>
              <div>Servings: {{recipe.servings}}</div>
              <img v-if="recipe.vegan==true" src="../images/pngguru.com.png" height="50" width="50" />
              <img
                v-if="recipe.vegetarian==true"
                src="../images/hiclipart.com.png"
                height="50"
                width="50"
              />
              <img
                v-if="recipe.glutenFree==true"
                src="../images/marshmallow-on-stick-free-png-8-original.png"
                height="50"
                width="50"
              />
              <div></div>
            </div>Ingredients:
            <ul>
              <li
                v-for=" r in recipe.extendedIngredients"
                :key="r.id"
              >{{ r.name }} - {{r.amount}} {{r.unit}}</li>
            </ul>
          </div>
          <div class="wrapped">
            Instructions:
            <ol>
              <li v-for="s in recipe._instructions" :key="s.number">{{ s.step }}</li>
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
  methods: {
    async addfave() {
      if (this.$root.store.username) {
        const res1 = await this.axios.post(
          "https://recipestest1.herokuapp.com/user/addFavoriteRecipe",
          {
            id: this.$route.params.recipeId
          }
        );
        alert("added");
      } else {
        alert("cant add");
      }
    }
  },
  async created() {
    try {
      let response;

      try {
        const id = this.$route.params.recipeId;
        let url = "https://recipestest1.herokuapp.com/recipes/recipepage/" + id;
        response = await this.axios.get(url);

        if (response.status !== 200) this.$router.replace("/NotFound");
        if (this.$root.store.username) {
          const res = await this.axios.post(
            "https://recipestest1.herokuapp.com/user/addWatch",
            {
              id: id
            }
          );
          //const res =  await this.axios.get("https://recipestest1.herokuapp.com/user/addWatch/" + this.$route.params.recipeId)
        }
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }

      let {
        analyzedInstructions,
        instructions,
        extendedIngredients,
        aggregateLikes,
        readyInMinutes,
        vegetarian,
        glutenFree,
        vegan,
        image,
        servings,
        title
      } = response.data;

      let _instructions = analyzedInstructions
        .map(fstep => {
          fstep.steps[0].step = fstep.name + fstep.steps[0].step;
          return fstep.steps;
        })
        .reduce((a, b) => [...a, ...b], []);

      let _recipe = {
        instructions,
        _instructions,
        vegetarian,
        glutenFree,
        vegan,
        extendedIngredients,
        aggregateLikes,
        readyInMinutes,
        image,
        servings,
        title
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
