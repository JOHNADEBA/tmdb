<template>
  <div class="movies-container">
    <p class="errors" v-if="errors">{{ errors }}</p>
    <div class="spinner" v-if="loading">
      <i class="fas fa-spinner fa-pulse"></i>
    </div>
    <div
      v-else
      class="movie-container"
      v-for="(movie, index) in allMovies"
      :key="index"
    >
      <img
        class="poster"
        :src="imgBaseUrl + movie.poster_path"
        alt="movie.original_title"
      />
      <h5>{{ movie.title }}</h5>
      <p class="date">{{ convertDate(movie.release_date) }}</p>
      <div class="vote">
        <div class="vote-color">
          <ve-progress
            :progress="movie.vote_average * 10"
            :data="circles"
            :angle="-90"
            :color="convertColor(movie.vote_average)"
            :empty-color="convertBackgroundColor(movie.vote_average)"
            empty-color-fill="black"
            :size="50"
            :thickness="3"
            empty-thickness="5%"
            line-mode="normal"
            line-position="in "
            empty-line-position="in"
            :hide-legend="false"
            :legend="movie.vote_average * 10"
            :legend-formatter="
              ({ currentValue }) =>
                `${new Intl.NumberFormat('en-US').format(currentValue)} `
            "
            legend-class="legend-custom-style"
            dash="60 0.9"
            animation="700 400"
            :noData="false"
            :loading="false"
            :loader="convertColor(movie.vote_average)"
            font-color="white"
            :half="false"
            :gap="10"
            font-size="0.9rem"
          >
          </ve-progress>
          <p class="percent">%</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import { VeProgress } from "vue-ellipse-progress";

export default {
  components: {
    VeProgress,
  },
  name: "Movies",
  emits: ["load"],
  props: ["allMovies", "loading", "errors"],
  data() {
    return {
      imgBaseUrl: "https://image.tmdb.org/t/p/w500",
    };
  },

  methods: {
    convertDate(dateString) {
      return moment(dateString).format("DD MMM YYYY");
    },
    convertColor(x) {
      if (x >= 7) return "rgb(6, 214, 6, 0.7)";
      else if (x >= 4 && x < 7) return "rgb(247, 247, 31, 0.7)";
      else if (x >= 0 && x < 4) return "rgb(126, 125, 125, 0.7)";
    },
    convertBackgroundColor(x) {
      if (x >= 7) return "rgb(6, 214, 6, 0.3)";
      else if (x >= 4 && x < 7) return "rgb(247, 247, 31, 0.3)";
      else if (x >= 0 && x < 4) return "rgb(126, 125, 125, 0.3)";
    },
  },
};
</script>

<style lang='scss'>
@import "@/assets/_shared.scss";

.spinner {
  font-weight: bolder;
  font-size: 5rem;
  margin: 15rem auto;
}

.errors {
  margin: 1rem;
}

.movies-container {
  @include bigFlexBetweenAlign;
  margin: 3rem 0 0 1rem;
  flex-wrap: wrap;
}
.movie-container {
  width: 18%;
  height: 28rem;
  border: $side-border;
  border-radius: $border-radius;
  box-shadow: $shadow;
  margin: 0.5rem;
  padding-bottom: 1rem;
  position: relative;
  cursor: pointer;

  & .poster {
    width: 100%;
    height: 70%;
    border-top-left-radius: $border-radius;
    border-top-right-radius: $border-radius;
  }

  & h5 {
    margin: 0 0.5rem;
    margin-top: 1.3rem;
    font-weight: bolder;
    font-size: 1rem;
  }

  & .date {
    margin: 0.3rem 0.2rem;
    font-size: 1rem;
    padding: 0 0.5rem;
    opacity: 0.6;
  }

  & .vote {
    @include bigFlexCenter;
    position: absolute;
    bottom: 7.8rem;
    left: 1rem;
    align-items: center;

    & .vote-color {
      position: relative;

      & .percent {
        position: absolute;
        top: 1rem;
        left: 2rem;
        font-size: 0.5rem;
        color: $bg-white;
      }
    }
  }
}

@media screen and (max-width: 1327px) {
  .movie-container {
    width: 22%;
  }
}
@media screen and (max-width: 1153px) {
  .movie-container {
    width: 30%;
  }
}
@media screen and (max-width: 955px) {
  .movie-container {
    width: 45%;
  }
}
@media screen and (max-width: 755px) {
  .movie-container {
    width: 70%;
    margin-left: 3rem;
  }
}
</style>