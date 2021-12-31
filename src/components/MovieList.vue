<template>
  <li v-for="movielist in movielists"
  v-bind:key="movielist.imdbID">
    <div class="movieList"
          @click.prevent="movieModal(movielist.imdbID)">
      <img :src="movielist.Poster" onerror="this.src='https://i.imgur.com/1OpTile.png';" />
      <div class="movieTitle">{{ movielist.Title }},</div>
      <div>{{ movielist.Year }}</div>
    </div>
    <MovieDetail :details="this.details" v-if="modal" @click.prevent="modalClose"/>
  </li>

</template>
<script>
import MovieDetail from './MovieDetail'

export default {
 props: {
   movielists: Object
 },
 components: {
   MovieDetail
 },
 data() {
    return {
      details: [],
      modal: false
    }
  },
  methods: {
    stateInit() {
      this.details = []
      this.modal = false
    },
   movieDetail: async (key) => {
     const movieDetaildata = await fetch(`https://www.omdbapi.com?apikey=7035c60c&i=${key}&plot=full
    `, { method: "GET" }).then(res => res.json())
    return movieDetaildata
   },
   async movieModal(key) {
     this.details = await this.movieDetail(key)
     this.modal = true

   },
   modalClose() {
     this.modal = false
   }
 }
}
</script>

<style lang="scss" scoped>
.movieList {
  display: grid;
  grid-template-columns: minmax(3, 1fr);
  gap: 0.5rem;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(16px) saturate(180%);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 12px;
  border: 1px solid rgba(209, 213, 219, 0.3);
  font-size: 0.8rem;
  padding: 0.5rem;
  overflow: inherit;
  text-align: center;
  img {
    height: 10rem;
    width: 8rem;
    border-radius: 15px;
    text-align: center;
  }
}
.movieTitle {
  width: 8rem;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  font-weight: 600;
}
</style>
