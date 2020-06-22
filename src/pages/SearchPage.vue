<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
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
    <Re>
  </div>
</template>

<script>
import cuisines from "../assets/cuisine";
import diets from "../assets/diets";
import intolerences from "../assets/intolrence";
export default {
  name: "Search",
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
      
    };
  },
  mounted() {
    // console.log("mounted");
    this.intolerences.push(...intolerences);
    this.diets.push(...diets);
    this.cuisines.push(...cuisines);
    // console.log($v);
  },
  methods: {
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
.container {
  max-width: 400px;
}
</style>
