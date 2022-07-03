<template>
  <div class="home">
      <section class="logo">
        <img :src="uofu" alt="U of u logo" width="100">
        <h1>  Megaplex</h1>
      </section>
        <div class="input-group mb-3">
          <input type="text" class="form-control" aria-label="Text input with dropdown button" placeholder="Search movies" v-model="searchTerm" @keyup.enter="doSearch($event)">
    </div>
    
    <div  class="filter" v-if="movies.length">
    <p class="filterP">Filter: </p>
    <select name="year" v-model="year" class="selectOne" @change="filterByYear(year)">
      <option v-for="movie in movies" :key="movie.imdbID" :value="movie.Year">{{movie.Year}}</option>
    </select>
    <button @click="year = null" class="unfilter" v-if="year">Unfilter</button>
    </div>
   <div class="navigate" v-if="movies.length">
   <button v-if="page > 1" @click="backPage(); getData(searchTerm,page)" class="btn btn-outline-dark"><svg xmlns="http://www.w3.org/2000/svg" height="18px" viewBox="0 0 24 24" width="18px" fill="#000000"><path d="M0 0h24v24H0z" fill="none"/><path d="M11.67 3.87L9.9 2.1 0 12l9.9 9.9 1.77-1.77L3.54 12z"/></svg></button>


   <button @click="nextPage(); getData(searchTerm, page)" class="btn btn-outline-dark next"><svg xmlns="http://www.w3.org/2000/svg" enable-background="new 0 0 20 20" height="18px" viewBox="0 0 20 20" width="18px" fill="#000000"><g><g><rect fill="none" height="20" width="20"/></g></g><g><polygon points="4.59,16.59 6,18 14,10 6,2 4.59,3.41 11.17,10"/></g></svg></button>

   
   </div>
    <div v-if="movieByYear.length && year !== null" class="movie-container" >
      <div v-for="item in movieByYear" :key="item.imdbID" class="each-movie">
      <div class="card" style="width: 14rem;">
        <img class="card-img-top" :src="item.Poster" :alt="item.Title">
          <div class="card-body">
            <h6 class="card-title">{{item.Title}}</h6>
            <p class="card-text">{{item.Year}}</p>
      </div>
      </div>

      </div>
    </div>
    <div v-else-if="movies.length && year == null" class="movie-container">
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
    <div v-else>
   <p class="message">Type a movie name to watch at U of U Megaplex</p>
    
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
    var page = ref(1)
    const type = ref([])
    const searchTerm = ref(null)
    const year = ref(null)
    const movies = ref([])
    const noData = ref(null)
    const movieByYear = ref([])
    const getData = async (searched, pageNum)=> {
      try {

  let fetched = await axios({
  method: 'GET',
  url: 'https://movie-database-alternative.p.rapidapi.com/',
  params: {s: `${searched}`, r: 'json', page: `${pageNum}`},
  headers: {
    'X-RapidAPI-Key': '783a1338cbmsh8f9ca0a70213a77p187916jsnb7ecdaf85ec1',
    'X-RapidAPI-Host': 'movie-database-alternative.p.rapidapi.com'
  }
})

if(fetched.status === 200){
  movies.value = fetched.data.Search
  // console.log(fetched.data.Search)
}
  

      } catch (err){
        console.log(err)
        noData.value = err.message
      }
    }
  


const doSearch = (e)=> {
  getData(e.target.value,page.value)
}

const nextPage = ()=> page.value++
const backPage =()=> page.value > 1 ? page.value-- : null

const filterByYear = (selectedYear)=> {
    movieByYear.value = movies.value.filter((movie)=> {
      return movie.Year == selectedYear
    })
}




return {uofu, getData, movies, year, searchTerm, doSearch, type, page, nextPage, backPage, movieByYear, filterByYear}

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
  justify-content: space-evenly;
  align-items: center;
}

.movie-container:hover {
  cursor: pointer;
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

.navigate {
  background: beige;
  display: flex;
  justify-content: space-between;
  margin-bottom: 5px;
}

.unfilter {
  margin-left: 5px;
  border-radius: 10px;
  color: white;
  background: red;
  font-weight: bold;
}

.message {
  width: 200px;
  height: 120px;
  background-color: transparent;
  position: relative;
  animation-name: move;
  animation-duration: 4s;
  animation-iteration-count: 1;
  border-radius: 10px;
  font-weight: bolder;
  text-decoration: double;
  padding: 5px;
  
}

@keyframes move {
  0%   {background-color:rgb(205, 123, 123); left:0px; top:0px;transform: rotate(20deg);}
  25%  {background-color:rgb(174, 174, 119); left:900px; top:0px;transform: rotate(40deg);}
  50%  {background-color:rgb(181, 181, 220); left:500px; top:500px;transform: rotate(80deg);}
  75%  {background-color:rgb(144, 212, 144); left:90px; top:200px;transform: rotate(120deg);}
  100% {background-color:rgb(232, 196, 196); left:0px; top:0px;transform: rotate(360deg);}
}
</style>