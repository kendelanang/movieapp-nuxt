<template>
  <!--Loading-->
  <Loading v-if="$fetchState.pending" />

  <div
    v-else
    class="container text-white min-h-[100vh] flex flex-col justify-center py-8 px-4"
  >
    <NuxtLink
      class="w-fit self-start mb-8 flex no-underline text-inherit items-center px-2 bg-[#c92502] rounded-md cursor-pointer transition-all hover:bg-[#891b02] text-white"
      :to="{ name: 'index' }"
      >Back</NuxtLink
    >
    <div
      class="movie-info flex flex-col items-center gap-8 text-white md:flex-row md:items-start"
    >
      <div class="movie-img">
        <img
          :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
          alt=""
          class="max-h-[500px] w-full md:max-h-[700px] md:w-full"
        />
      </div>
      <div class="movie-content">
        <h1 class="text-5xl font-normal">Title {{ movie.title }}</h1>
        <p class="mt-3 text-xl leading-6 italic">
          <span class="font-semibold underline">Tagline : </span> "{{
            movie.tagline
          }}"
        </p>
        <p>
          <span
            >Released :
            {{
              new Date(movie.release_date).toLocaleString('en-us', {
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </span>
        </p>
        <p>
          <span>Duration : {{ movie.runtime }} minutes</span>
        </p>
        <p>
          <span>Revenue : </span>
          {{
            movie.revenue.toLocaleString('en-us', {
              style: 'currency',
              currency: 'USD',
            })
          }}
        </p>
        <p>
          <span>Overview : {{ movie.overview }}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'SingleMovie',

  data() {
    return {
      movie: '',
    }
  },

  async fetch() {
    await this.getSingleMovie()
  },
  fetchDelay: 1000,

  head() {
    return {
      title: this.movie.title,
    }
  },

  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=723611760df2f88b05c9cdbbb7d845e3&language=en-US`
      )
      const result = await data
      this.movie = result.data
    },
  },
}
</script>

<style lang="scss" scoped></style>
