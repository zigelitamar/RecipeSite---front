<template>
  <div>
    <b-card :title="recipe.title" tag="article" style="max-width: 20rem;" class="mb-2">
      <router-link :to="{ name: 'recipe', params: { recipeId: recipe.id } }" class="recipe-preview">
        <b-card-img :src="recipe.image" alt="Image" top />
      </router-link>
      <div>
        <b-button @click="addfave">
          <b-icon-heart v-if="recipe.favorite==false " font-scale="2" />
        </b-button>
        <b-icon-heart-fill v-if="recipe.favorite==true " font-scale="2" animation="throb" />

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
            <img
              src="../images/marshmallow-on-stick-free-png-8-original.png"
              height="50"
              width="50"
            />
          </li>
        </ul>
      </b-card-text>
    </b-card>
  </div>
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
              id: this.recipe.id
            }
          );
          alert("added");
        } else {
          alert("cant add");
        }
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>

<style scoped>
.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}
</style>
