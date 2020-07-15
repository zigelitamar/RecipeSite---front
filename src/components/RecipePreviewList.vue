<template>
  <b-container>
    <div class="containers">
      <!-- <h2>
        {{ title }}:
        <slot></slot>
      </h2>-->
    </div>
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
        } else if (type == "favorite" && this.$root.store.username) {
          endpoint = "https://recipestest1.herokuapp.com/user/getFavorites";
        }
        const response = await this.axios.get(endpoint, {
          withCredentials: true
        });
        const recipes = response.data;

        recipes.forEach(recipe => {
          recipe.watched = false;
          recipe.favorite = false;
        });

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
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
h2 {
  font-size: 60px;
  line-height: 100px;
  font-family: Helvetica, sans-serif;
  font-weight: bold;
  text-align: center;
  text-shadow: rgba(0, 0, 0, 0.3) 5px 5px 5px;
}

#containers {
  /*gradient*/
  background: #666666; /* old browsers */
  background: -moz-linear-gradient(
    top,
    #666666 4%,
    #545454 50%,
    #3a3a3a 51%,
    #131313 100%
  ); /* firefox */

  background: -webkit-gradient(
    linear,
    left top,
    left bottom,
    color-stop(4%, #666666),
    color-stop(50%, #545454),
    color-stop(51%, #3a3a3a),
    color-stop(100%, #131313)
  ); /* webkit */

  filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#666666', endColorstr='#131313',GradientType=0 ); /* ie */

  /*box styles*/
  height: 200px;
  width: 400px;
  margin: 50px auto;

  /*shadow*/
  -webkit-box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.3);
  -moz-box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.3);
  box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.3);

  /*corners*/
  -webkit-border-radius: 50px;
  -moz-border-radius: 50px;
  border-radius: 50px;
}
</style>
