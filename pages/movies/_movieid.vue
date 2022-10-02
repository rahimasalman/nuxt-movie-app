<template>
  <div>
    <Loading v-if="$fetchState.pending" />
    <!-- Movie info-->
    <div
      v-else
      class="single-movie container">
      <NuxtLink class="button" :to="{ name: 'index' }"> Back </NuxtLink>
      <div class="movie-info">
        <div class="movie-img">
          <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt=""/>
        </div>
        <div class="movie-content">
          <h1> Title: &nbsp;{{movie.title}}</h1>
          <p class="movie-fact tagline">
            <span>Tagline:</span>&nbsp;
            "{{movie.tagline}}"
          </p>
          <p class="movie-fact">
            <span> Released: </span>&nbsp;
            {{
              new Date(movie.release_date).toLocaleString('en-us', {
                day: 'numeric',
                month: 'long',
                year: 'numeric'
              })
            }}
          </p>
          <p class="movie-fact">
            <span>Duration:</span>&nbsp; {{movie.runtime}} minutes
          </p>
          <p class="movie-fact">
           <span>Revenue:</span>&nbsp;
            {{
              movie.revenue.toLocaleString('en-us', {
                style: 'currency',
                currency: 'USD',
              })
            }}
          </p>
          <p class="movie-fact">
            <span>Overview:</span> &nbsp;{{movie.overview}}
          </p>
        </div>
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
  // delay for fetching
  fetchDelay: 1000,
  head() {
    return {
      title: this.movie.title,
    }
  },
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=a162458fb76b73d7c6c4960124789d6e&language=en-US`
      )
      const result = await data
      this.movie = result.data
    },
  },
}
</script>

<style lang="scss">
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 300px;
        width: 100%;
        @media (min-width: 600px) {
          max-height: 500px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>
