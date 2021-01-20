<template>
  <BContainer>
    <h3 class="list-title">{{listTitle}}</h3>
    <BRow>
      <template v-if="isExist">
        <BCol cols="3" v-for="(movie, key) in list" :key="key">
          <MovieItem
              :movie="movie"
              @mouseover.native="onMouseOver(movie.Poster)"
              @removeItem="onRemoveItem"
              @showModal="onShowMovieInfo"
          />
        </BCol>
      </template>
      <template v-else>
        <div>список пуст :(</div>
      </template>
    </BRow>
    <BModal body-class="movie-modal-body" :id="movieInfoModalID" size="xl" hide-footer hide-header>
      <MovieInfoModalContent :movie="selectedMovie" @closeModal="onCloseModal"/>
    </BModal>
  </BContainer>
</template>

<script>
import {mapActions, mapGetters} from 'vuex'
import MovieItem from "@/components/MovieItem";
import MovieInfoModalContent from "@/components/MovieInfoModalContent";
export default {
  name: "MoviesList",
  components: {
    MovieInfoModalContent,
    MovieItem,
  },
  data: () => ({
    movieInfoModalID : 'movie-info',
    selectedMovieID: ''
  }),
  props: {
    list : {
      type: Object,
      default: () => ({})
    }
  },
  computed: {
    ...mapGetters('movies', ['isSearch']),
    isExist(){
      return Boolean(Object.keys(this.list).length)
    },
    listTitle(){
      return this.isSearch ? 'Результат поиска' : 'IMDB Top 250'
    },
    selectedMovie(){
      return this.selectedMovieID ? this.list[this.selectedMovieID]: null
    }
  },
  methods: {
    ...mapActions('movies', ['removeMovie']),
    ...mapActions(['showNotify']),
    onMouseOver(poster){
      this.$emit('changePoster',poster)
    },
    async onRemoveItem({id, title}){
      const isConfirmed = await this.$bvModal.msgBoxConfirm(`Вы уверены, что хотите удалить ${title}?`)
      console.log(isConfirmed);
      if (isConfirmed){
        this.removeMovie(id)
        this.showNotify({
          message: 'Фильм удален успешно',
          variant: 'success',
          title: 'Действие выполнено'
        })
      }
    },
    onShowMovieInfo(id){
      console.log(id)
      this.selectedMovieID = id
      this.$bvModal.show(this.movieInfoModalID)
    },
    onCloseModal(){
      try {
        this.$bvModal.hide(this.movieInfoModalID)
        this.selectedMovieID = null
      }catch (error){
        console.log(error);
      }
    }
  }
};
</script>

<style lang="sass">
.list-title
  font-size: 50px
  margin-bottom: 30px
  color: aliceblue
.movie-modal-body
  padding: 0 !important
</style>
