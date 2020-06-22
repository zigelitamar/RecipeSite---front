<template>
  <div class="container">
    <b-container>
      <b-row>
        <b-col>
          <RecipePreviewList rType="rand" title="Randome Recipes" class="RandomRecipes center" />
        </b-col>
        <b-col>
          <router-link
            v-if="!$root.store.username"
            to="/login"
            tag="button"
          >You need to Login to vue this</router-link>
          <RecipePreviewList
            v-if="$root.store.username"
            title="Last Viewed Recipes"
            rType="lastSeen"
            class="RandomRecipes center"
          ></RecipePreviewList>
        </b-col>
      </b-row>
    </b-container>
    <!-- <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div>-->
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  components: {
    RecipePreviewList
  }
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
