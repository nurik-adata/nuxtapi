<template>
  <div>
    <input v-model="keyword" @input="getResults">
    <div>
      <router-link tag="div" to="/results">
        <button>Найти</button>
      </router-link>
    </div>
    <div v-bind:key="result.id" v-for="result in results">
      <p>Results are: {{ result.year }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  watch:{
    keyword:"getResults"
  },
  methods:{
    getResults(){
      if(this.keyword.length > 0)
        axios.get("http://localhost:3000/rows/?name="+this.keyword)
          .then(res=>(this.results = res.data))
          .catch(err => console.log(err));
    }
  },
  data(){
    return{
      keyword: '',
      results:[]
    }
  }
}

</script>

<style>

</style>
