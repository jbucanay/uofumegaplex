<template>
  <div class="home">
    <div class="input-group mb-3">
  <div class="input-group-prepend">
    <button class="btn btn-outline-secondary dropdown-toggle" type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Dropdown</button>
    <div class="dropdown-menu">
      <a class="dropdown-item" href="#">Action</a>
      <a class="dropdown-item" href="#">Another action</a>
      <a class="dropdown-item" href="#">Something else here</a>
      <div role="separator" class="dropdown-divider"></div>
      <a class="dropdown-item" href="#">Separated link</a>
    </div>
  </div>
  <input type="text" class="form-control" aria-label="Text input with dropdown button">
</div>
  
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import axios from 'axios'


export default {
  name: 'HomeView',

  setup(){
    const moveies = ref([])
    const noData = ref(null)
    const getData = async ()=> {
      try {

        let fetched = await axios({
  method: 'GET',
  url: 'https://movie-database-alternative.p.rapidapi.com/',
  params: {s: 'star wars', r: 'json', page: '1'},
  headers: {
    'X-RapidAPI-Key': '783a1338cbmsh8f9ca0a70213a77p187916jsnb7ecdaf85ec1',
    'X-RapidAPI-Host': 'movie-database-alternative.p.rapidapi.com'
  }
})
  console.log(fetched)

      } catch (err){
        console.log(err)
        noData.value = err.message
      }
    }
  
  getData()



  }
 
}
</script>
