<template>
  <div class="home">
    <!--Hero-->
    <Hero />

    <!--Search-->
    <div class="container search flex py-8 px-4">
      <input
        v-model.lazy="searchInput"
        type="text"
        placeholder="Search"
        class="w-full max-w-xs py-3 px-2 text-sm border-none focus:outline-none"
        @keyup.enter="$fetch"
      />
      <div
        v-show="searchInput !== ''"
        class="ml-4 flex no-underline text-inherit items-center px-2 bg-[#c92502] rounded-md cursor-pointer transition-all hover:bg-[#891b02] text-white"
        @click="clearSearch"
      >
        Clear Search
      </div>
    </div>

    <!-- Loading Animation -->
    <Loading v-if="$fetchState.pending" class="p-32 items-center" />

    <!--Movies when search-->
    <div v-else class="container py-8 px-4">
      <!--Search Result-->
      <div
        v-if="searchInput !== ''"
        id="movie-grid"
        class="movie-grid grid gap-x-8 gap-y-16 grid-cols-1 md:grid-cols-2 lg:grid-cols-4"
      >
        <div
          v-for="(movie, index) in searchedMovies"
          :key="index"
          class="movie relative flex flex-col"
        >
          <div class="movie-img relative overflow-hidden group">
            <img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
              class="block w-full h-full group-hover:opacity-75 transition-all group-hover:scale-125"
            />
            <p
              class="review absolute top-0 left-0 flex justify-center items-center w-10 h-10 bg-[#c92502] text-white rounded-b-2xl shadow-md"
            >
              {{ movie.vote_average }}
            </p>
            <p
              class="overview leading-5 absolute bottom-0 bg-red-900 p-3 text-white translate-y-full transition-all group-hover:translate-y-0"
            >
              {{ movie.overview }}
            </p>
          </div>
          <div class="info mt-auto">
            <p class="title mt-2 text-white text-xl">
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release mt-2 text-[#c9c9c9]">
              Released :
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink
              class="mt-2 inline-block no-underline text-inherit py-2 px-4 bg-[#c92502] rounded-md cursor-pointer transition-all hover:bg-[#891b02] text-white bg-transparent border border-solid border-[#c92502]"
              :to="{ name: 'movies-movieid', params: { id: movie.id } }"
              >Get More Info</NuxtLink
            >
          </div>
        </div>
      </div>

      <!--Movie Result-->
      <div
        id="movie-grid"
        class="movie-grid grid gap-x-8 gap-y-16 grid-cols-1 md:grid-cols-2 lg:grid-cols-4"
      >
        <div
          v-for="(movie, index) in movies"
          :key="index"
          class="movie relative flex flex-col"
        >
          <div class="movie-img relative overflow-hidden group">
            <img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
              class="block w-full h-full group-hover:opacity-75 transition-all group-hover:scale-125"
            />
            <p
              class="review absolute top-0 left-0 flex justify-center items-center w-10 h-10 bg-[#c92502] text-white rounded-b-2xl shadow-md"
            >
              {{ movie.vote_average }}
            </p>
            <p
              class="overview leading-5 absolute bottom-0 bg-red-900 p-3 text-white translate-y-full transition-all group-hover:translate-y-0"
            >
              {{ movie.overview }}
            </p>
          </div>
          <div class="info mt-auto">
            <p class="title mt-2 text-white text-xl">
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release mt-2 text-[#c9c9c9]">
              Released :
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink
              class="mt-2 inline-block no-underline text-inherit py-2 px-4 bg-[#c92502] rounded-md cursor-pointer transition-all hover:bg-[#891b02] text-white bg-transparent border border-solid border-[#c92502]"
              :to="{ name: 'movies-movieid', params: { id: movie.id } }"
              >Get More Info</NuxtLink
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'home',

  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: '',
    }
  },

  async fetch() {
    if (this.searchInput === '') {
      await this.getMovies()
      return
    }
    if (this.searchInput !== '') {
      await this.searchMovies()
    }
  },
  fetchDelay: 1000,

  head() {
    return {
      title: 'Movie App - Latest Streaming Movie Info',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, stremaing',
        },
      ],
    }
  },

  watch: {
    searchInput() {
      console.log(this.searchInput)
    },
  },

  methods: {
    async getMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=723611760df2f88b05c9cdbbb7d845e3&language=en-US&page=1`
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.movies.push(movie)
      })
    },

    async searchMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=723611760df2f88b05c9cdbbb7d845e3&language=en-US&page=1&query=${this.searchInput}`
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie)
      })
    },

    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    },
  },
}
</script>
