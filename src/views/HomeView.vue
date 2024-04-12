import { RouterLink } from 'vue-router';

<template>
  <div class="home">
    <div class="feature-card">
      <RouterLink to="/movie/tt0409591">
        <img
          src=https://m.media-amazon.com/images/M/MV5BZmQ5NGFiNWEtMmMyMC00MDdiLTg4YjktOGY5Yzc2MDUxMTE1XkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_QL75_UY281_CR1,0,190,281_.jpg
          alt="Poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>Naruto</h3>
          <p>Naruto movie description</p>
        </div>
      </RouterLink>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for?" v-model="search"/>
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" 
      >
        <!-- {{ movie.Title }} -->
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import {options} from '@/env';
export default {
  setup(){
    const search = ref("");
    const movies = ref([])

    const SearchMovies = () => {
      if (search.value != "" ){
        fetch(`https://api.themoviedb.org/3/search/multi?query=${search.value}&page=2`, options)
        .then(res => res.json())
        .then(res => {
          movies.value = res.results;
          search.value = "";
          console.log(movies.value);
          })
          
        .catch(err => console.error(err))
      }
    }

    return {
      search,
      movies, 
      SearchMovies
    }
  }
}
</script>

<style lang="scss">
.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;

      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #fff;
        margin-bottom: 16px;
      }

      p {
        color: #fff;
      }
    }
  }

  .search-box{
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input{
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"]{
        width: 100%;
        color: #FFF;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder{
          color: #f3f3f3;
        }

        &:focus{
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"]{
        width: 100%;
        max-width: 300px;
        background-color: #42B883;
        padding: 16px;
        border-radius: 8px;
        color: #FFF;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3B8070;
        }
      }
    }
  }
}
</style>
