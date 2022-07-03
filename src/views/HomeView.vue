<template>
  <div class="home">
      <section class="logo">
        <img :src="uofu" alt="U of u logo" width="100">
        <h1>  Megaplex</h1>
      </section>
        <div class="input-group mb-3">
          <input type="text" class="form-control" aria-label="Text input with dropdown button" placeholder="Search movies" v-model="searchTerm" @keyup.enter="doSearch($event)">
    </div>
    <!-- v-if="movies.length" -->
    <div  class="filter">
    <p class="filterP">Filter: </p>
    <select name="year" v-model="year" @change="movieByYear" class="selectOne">
      <option v-for="movie in movies" :key="movie.imdbID" :value="movie.Year">{{movie.Year}}</option>
    </select>
    <select name="type" v-model="type" @change="movieByType" class="selectTwo">
      <option v-for="movie in movies" :key="movie.imdbID" :value="movie.Type">{{movie.Type}}</option>
    </select>
    </div>
   <div class="navigate">
   <button type="button" class="btn btn-outline-dark"><svg xmlns="http://www.w3.org/2000/svg" height="18px" viewBox="0 0 24 24" width="18px" fill="#000000"><path d="M0 0h24v24H0z" fill="none"/><path d="M11.67 3.87L9.9 2.1 0 12l9.9 9.9 1.77-1.77L3.54 12z"/></svg></button>


   <button type="button" class="btn btn-outline-dark"><svg xmlns="http://www.w3.org/2000/svg" enable-background="new 0 0 20 20" height="18px" viewBox="0 0 20 20" width="18px" fill="#000000"><g><g><rect fill="none" height="20" width="20"/></g></g><g><polygon points="4.59,16.59 6,18 14,10 6,2 4.59,3.41 11.17,10"/></g></svg></button>


   </div>

    <div v-if="movies.length" class="movie-container">
    <div v-for="movie in movies" :key="movie.imdbID" class="each-movie">
    <div class="card" style="width: 14rem;">
        <img class="card-img-top" :src="movie.Poster" alt="Movie poster">
        <div class="card-body">
          <h6 class="card-title">{{movie.Title}}</h6>
          <p class="card-text">{{movie.Year}}</p>
        </div>
    </div>

    </div>

    </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import axios from 'axios'


export default {
  name: 'HomeView',

  setup(){
    const uofu = ref('https://upload.wikimedia.org/wikipedia/commons/thumb/9/92/Utah_Utes_-_U_logo.svg/1121px-Utah_Utes_-_U_logo.svg.png')
    const type = ref([])
    const searchTerm = ref(null)
    const year = ref(null)
    const movies = ref([])
    const noData = ref(null)
    const getData = async (searched)=> {
      try {

  let fetched = await axios({
  method: 'GET',
  url: 'https://movie-database-alternative.p.rapidapi.com/',
  params: {s: `${searched}`, r: 'json', page: '2'},
  headers: {
    'X-RapidAPI-Key': '783a1338cbmsh8f9ca0a70213a77p187916jsnb7ecdaf85ec1',
    'X-RapidAPI-Host': 'movie-database-alternative.p.rapidapi.com'
  }
})

if(fetched.status === 200){
  movies.value = fetched.data.Search
  console.log(fetched.data.Search)
}
  

      } catch (err){
        console.log(err)
        noData.value = err.message
      }
    }
  


const doSearch = (e)=> {
  getData(e.target.value)
}

// const filterByYear = movies.value.filter((movie => {
//   return movie.year === year
// })



return {uofu, getData, movies, year, searchTerm, doSearch, type}

  }


 
}
</script>

<style scoped>
.logo {
  display: flex;
  align-items: baseline;
  margin-bottom: 2em;
}

.home {
  margin: 10px 5em 0 5em;
}

.movie-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
}

.card-title {
  font-weight: 600;
}

.filter {
  display: flex;
  margin-top: -1%;
  margin-bottom: 2em;

}

.filterP {
  color: red;
  margin-right: 10px
}

.selectOne, .selectTwo {
  background: transparent;
  padding: 5px;
  border-radius: 4px;
  text-indent: 0.01px;
}

.selectOne {
  margin-right: 5px;
}
</style>