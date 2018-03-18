<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}

    spinner(v-show="isLoading")

    ul
      artist(v-for="artist in artists" v-bind:artist = "artist" v-bind:key = "artist.mbid")
</template>

<script>
import Artist from './Components/Artists.vue'
import Spinner from './Components/Spinner.vue'
import getArtists from './api/'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        { name: 'Argentina', value: 'argentina' },
        { name: 'Colombia', value: 'colombia' },
        { name: 'España', value: 'spain' }
      ],
      selectedCountry: 'colombia',
      isLoading : true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods:{
    refreshArtists(){
      const self = this
      self.isLoading = true
      self.artists = []
      getArtists(this.selectedCountry)
      .then(function (artists){
        self.artists = artists
        self.isLoading = false
      })
    }
  },
  mounted(){
    this.refreshArtists()
  },
  watch:{
    selectedCountry(){
      this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1,h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
