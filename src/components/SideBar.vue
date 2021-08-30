<template>
  <h2>Popular Movies</h2>
  <div class="filters">
    <div class="filters-head" @click="this.showAccordion = !this.showAccordion">
      <h3>Filters</h3>
      <h3 v-if="showAccordion"><i class="fas fa-angle-down"></i></h3>
      <h3 v-else><i class="fas fa-angle-right"></i></h3>
    </div>

    <div class="filters-genre" v-if="showAccordion">
      <p class="genres-header">Genres</p>
      <div v-if="allGenres.length > 0" class="filters-div">
        <div v-for="(genre, index) in allGenres" :key="index">
          <button
            class="filters-btn"
            @click="emitGenre(genre.id, genre)"
            v-bind:class="{ selected: isSelected.includes(genre) }"
          >
            {{ genre.name }}
          </button>
        </div>
      </div>
    </div>
  </div>
  <div>
    <button
      :class="{ search_btn_active: isSearch }"
      class="search-btn"
      @click="getAllSearchGenre()"
    >
      Search
    </button>
  </div>
</template>

<script>
export default {
  emits: ["name", "search"],
  props: ["allGenres"],
  data() {
    return {
      showAccordion: false,
      isSelected: [],
      isSearch: false,
    };
  },
  methods: {
    emitGenre(genre, name) {
      this.isSearch = true;

      const index = this.isSelected.indexOf(name);
      if (index >= 0) this.isSelected.splice(index, 1);
      else this.isSelected.push(name);
      this.$emit("name", genre);
    },
    getAllSearchGenre() {
      this.isSearch = false;
      this.$emit("search");
    },
  },
};
</script>

<style lang="scss">
@import "@/assets/_shared.scss";

.filters {
  width: 100%;
  border: $side-border;
  border-radius: $border-radius;
  box-shadow: $shadow;
  margin-bottom: 1.3rem;

  & .filters-head {
    @include bigFlexBetweenAlign;
    padding: 0.8rem 1.5rem;
  }
  & .filters-genre {
    width: 100%;
    margin-bottom: 1rem;
    border-top: $inside-border;

    & .genres-header {
      margin: 1rem;
      font-family: Arial, sans-serif;
    }
    & .filters-div {
      width: 81%;
      display: flex;
      align-items: center;
      flex-wrap: wrap;
      margin-left: 1rem;
    }
  }
}
.filters-btn {
  cursor: pointer;
  border: solid 0.1rem gray;
  background: $bg-white;
  border-radius: 1.2rem;
  padding: 0.3rem 0.9rem;
  margin: 0.3rem;
  margin-left: 0;
}
.filters-btn:hover,
.selected {
  background: $bg-btn-color;
  color: $bg-white;
  border: solid 0.1rem $bg-btn-color;
}
.search-btn {
  width: 100%;
  border-radius: 1.2rem;
  font-weight: bold;
  color: rgba(90, 89, 89, 0.7);
  font-size: 1rem;
  padding: 0.8rem 0.9rem;
  border: none;
  pointer-events: none;
}
.search_btn_active {
  pointer-events: visible;
  background: $bg-btn-color;
  cursor: pointer;
  color: $bg-white;

  &:hover {
    background: rgb(1, 35, 46);
    color: $bg-white;
  }
}
</style>