<template>
  <div class="movie-item mb-3">
    <div class="movie-item-poster" :style="posterBg"></div>
    <div class="movie-info-wrap d-flex">
      <div class="movie-item-info">
        <h3 class="movie-title">{{movie.Title}}</h3>
        <span class="movie-year">{{movie.Year}}</span>
      </div>
      <div class="movie-item-controls row no-gutters">
        <div class="col pr-2 pl-2">
          <BButton size="md"  block variant="outline-light">Изменить</BButton>
        </div>
        <div class="col">
          <BButton size="md"  block variant="outline-light" @click="emitRemoveEvent">Удалить</BButton>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MovieItem",
  props: {
    movie: {
      type: Object,
      required: true
    }
  },
  computed: {
    posterBg(){
      return{
        "background-image": `url(${this.movie.Poster})`
      }
    }
  },
  methods: {
    emitRemoveEvent(){
      this.$emit('removeItem', {id: this.movie.imdbID, title: this.movie.Title})
    }
  }
};
</script>

<style lang="sass" scoped>
.movie-title
  font-size: 20px
  color: #fff
.movie-year
  font-size: 14px
  color: #ffffff
.movie-item
  position: relative
  cursor: pointer
  border-radius: 5px
  overflow: hidden
  transition: all .3s ease-in
  height: 400px
  &:hover
    box-shadow: 0 5px 30px rgba(0, 0, 0, 0.7)
    transform: scale(1.02)
.movie-item-poster
  position: absolute
  top: 0
  left: 0
  right: 0
  bottom: 0
  z-index: -1
  background-repeat: no-repeat
  background-size: cover
  background-position: center center
.movie-info-wrap
  padding: 20px 10px
  flex-direction: column
  justify-content: space-between
  height: 100%
  opacity: 0
  transition: all 0.3s ease
  &:hover
    opacity: 1
    background-color: rgba(0, 0,0, .7)
</style>