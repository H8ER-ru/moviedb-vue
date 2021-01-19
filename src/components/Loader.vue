<template>
  <div class="loader-wrap" v-if="isShowLoader">
    <b-spinner type="grow" variant="light" label="loading..."></b-spinner>
  </div>
</template>

<script>
import {mapGetters} from 'vuex'
export default {
  name: "loader",
  computed: {
    ...mapGetters(['isShowLoader'])
  }

};
</script>

<style lang="sass" scoped>
.loader-wrap 
  position: fixed
  width: 100%
  height: 100%
  top: 0
  left: 0
  background-color: rgba(black , 0.7)
  display: flex
  justify-content: center
  align-items: center
  z-index: 9999
</style>