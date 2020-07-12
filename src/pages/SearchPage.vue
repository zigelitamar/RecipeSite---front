<template>
  <div>
    <h1 class="title">Search Page</h1>
    <div class="formdiv">
      <b-form @submit.prevent="onSearch">
        <b-form-group>
          <b-form-input id="searchQ" v-model="form.searchQ" type="text" placeholder="search..."></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-resultnum"
          label-cols-sm="3"
          label="Resaults:"
          label-for="resultnum"
        >
          <b-form-select id="resultnum" v-model="form.number" :options="resultNum"></b-form-select>
        </b-form-group>
        <b-form-group label="Choose your filters:">
          <b-form-select id="Diet" v-model="form.Diet" :options="diets"></b-form-select>

          <b-form-select id="Cuisine" v-model="form.Cuisine" :options="cuisines"></b-form-select>

          <b-form-select id="Intolerence" v-model="form.Intolerence" :options="intolerences"></b-form-select>
        </b-form-group>
        <b-button
          type="submit"
          variant="primary"
          style="width:100px;display:block;"
          class="mx-auto w-100"
        >Search</b-button>
      </b-form>
      <div v-if="flag">
        <b-button
          @click="sortby('time')"
          variant="primary"
          style="width:100px;display:block;"
          class="mx-auto w-100"
        >Sort by time</b-button>
        <b-button
          @click="sortby('pop')"
          variant="primary"
          style="width:100px;display:block;"
          class="mx-auto w-100"
        >Sort by popularity</b-button>
      </div>
    </div>

    <div v-if="flag">
      <ul :style="gridStyle" class="grid">
        <li v-for="r in recipes" :key="r.id" class="card">
          <RecipePreview :recipe="r" />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import cuisines from "../assets/cuisine";
import diets from "../assets/diets";
import intolerences from "../assets/intolrence";
import RecipePreview from "../components/RecipePreview";
export default {
  components: {
    RecipePreview
  },
  data() {
    return {
      form: {
        searchQ: "",
        number: 5,
        Diet: null,
        Cuisine: null,
        Intolerence: null
      },
      diets: [{ value: null, text: "diet", disabled: true }],
      cuisines: [{ value: null, text: "cuisine", disabled: true }],
      intolerences: [{ value: null, text: "intolerance", disabled: true }],
      resultNum: [5, 10, 15],
      flag: false,
      noresults: false,
      recipes: [],
      numberOfColumns: 3
    };
  },
  mounted() {
    // console.log("mounted");
    this.intolerences.push(...intolerences);
    this.diets.push(...diets);
    this.cuisines.push(...cuisines);
    // console.log($v);
  },
  computed: {
    gridStyle() {
      return {
        gridTemplateColumns: `repeat(${this.numberOfColumns}, minmax(100px, 1fr))`
      };
    }
  },
  methods: {
    sortby(el) {
      if (el == "time") {
        this.recipes.sort((a, b) => a.readyInMinutes - b.readyInMinutes);
        console.log(this.recipes);
      }
      if (el == "pop") {
        this.recipes.sort((a, b) => b.aggregateLikes - a.aggregateLikes);
        console.log(this.recipes);
      }
    },
    async Search() {
      try {
        let search_params = {};
        if (this.form.Diet != null) {
          search_params.diet = this.form.Diet;
        }
        if (this.form.Cuisine != null) {
          search_params.cuisine = this.form.Cuisine;
        }
        if (this.form.Intolerence != null) {
          search_params.intolerance = this.form.Intolerence;
        }

        const res = await this.axios.get(
          `https://recipestest1.herokuapp.com/recipes/search/query/${this.form.searchQ}/amount/${this.form.number}`,
          { params: search_params }
        );
        let all = res.data;
        console.log(all);
        this.recipes = [];
        this.recipes.push(...all);
        if (this.recipes.length > 0) {
          this.flag = true;
        } else {
          this.noresults = true;
        }
      } catch (err) {
        console.log(err.res);
        this.form.submitError = err.res.data.message;
      }
    },
    onSearch() {
      this.Search();
    }
  }
};
</script>

<style lang="scss" scoped>
.formdiv {
  margin: auto;
  position: relative;
  text-align: center;
  top: 50%;
  width: 20%;
  height: 400px;
  width: 500px;
}
.grid {
  display: grid;
  grid-gap: 1em;
}
.card {
  padding: 2em;
}
</style>
