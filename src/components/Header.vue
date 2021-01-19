<template>
  <header class="header">
    <BNavbar type="dark" variant="dark" class="navbar">
      <BContainer>
        <BNavbarBrand href="#">MovieDB</BNavbarBrand>
        <BNavForm>
          <b-form-input
            class="mr-sm-2 search-input" placeholder="Поиск"
            v-model="searchValue"
            debounce="500"
          />
        </BNavForm>
      </BContainer>
    </BNavbar>
  </header>
</template>

<script>
import {mapActions} from 'vuex'
export default {
  name: "Header",
  data: () =>({
    searchValue: ""
  }),
  watch:{
    searchValue: 'onSearchValueChange'
  },
  methods: {
    ...mapActions('movies', ['searchMovie', 'fetchMovies', 'toggleSearchState']),
    onSearchValueChange(value){
      if(value){
        this.searchMovie(value)
        this.toggleSearchState(true)
      }else {
        this.fetchMovies()
        this.toggleSearchState(false)
      }

    }
  }
};
</script>

<style lang="sass" scoped>
.header
  margin-bottom: 30px
.navbar
  background-color: rgba(0, 0,0, .7) !important
.search-input
  color: white
  background: rgba(white, 0.1)
  border-color: rgba(black, 0.6)
  &:focus
    color: #fff
    box-shadow: none
    background: rgba(white, 0.2)
    border-color: rgba(black, 0.6)
</style>