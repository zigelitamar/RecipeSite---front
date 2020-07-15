<template>
  <b-card
    border-variant="secondary"
    :header="recipe.title"
    tag="article"
    style="max-width: 20rem;"
    class="mb-2"
  >
    <router-link :to="{ name: 'recipe', params: { recipeId: recipe.id } }" class="recipe-preview">
      <b-card-img :src="recipe.image" top />
    </router-link>
    <div class="signs">
      <b-button id="toggle-btn" v-if="recipe.favorite==false" variant="light" @click="addfave">
        <b-icon-heart v-if="recipe.favorite==false " font-scale="2" />
      </b-button>
      <b-modal ref="my-modal" hide-footer title>
        <div class="d-block text-center">
          <h3>Oh no, This is exclusive content!</h3>
        </div>
        <br />
        <b-button class="mt-3" variant="outline-danger" block @click="Login">Login</b-button>
        <br />

        <b-button class="mt-2" variant="outline-danger" block @click="Register">Join us!</b-button>
      </b-modal>
      <div class="divider" />
      <b-icon-heart-fill v-if="recipe.favorite==true " font-scale="2" animation="throb" />
      <div class="divider" />
      <b-icon-eye-fill v-if="recipe.watched==true " font-scale="2" />
    </div>
    <b-card-text>
      <ul class="recipe-overview">
        <li>{{ recipe.readyInMinutes }} minutes</li>
        <li>{{ recipe.aggregateLikes }} likes</li>
        <li v-if="recipe.vegan==true">
          <img src="../images/pngguru.com.png" height="50" width="50" />
        </li>
        <li v-if="recipe.vegetarian==true">
          <img src="../images/hiclipart.com.png" height="50" width="50" />
        </li>
        <li v-if="recipe.glutenFree==true">
          <img src="../images/marshmallow-on-stick-free-png-8-original.png" height="50" width="50" />
        </li>
      </ul>
    </b-card-text>
  </b-card>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    recipe: {
      type: Object,
      required: true
    }
  },

  methods: {
    async addfave() {
      try {
        if (this.$root.store.username) {
          const res1 = await this.axios.post(
            "https://recipestest1.herokuapp.com/user/addFavoriteRecipe",
            {
              id: this.$props.recipe.id
            }
          );
          this.recipe.favorite = true;
          this.recipe.watched = true;
        } else {
          this.$refs["my-modal"].show();
        }
      } catch (err) {
        console.log(err);
      }
    },
    Login() {
      this.$router.push("/login").catch(() => {
        this.$forceUpdate();
      });
    },
    Register() {
      this.$router.push("/register").catch(() => {
        this.$forceUpdate();
      });
    }
  }
};
</script>

<style scoped>
.card {
  font-family: Verdana;
  font: bold;
}
.divider {
  width: 5px;
  height: auto;
  display: inline-block;
}
</style>
