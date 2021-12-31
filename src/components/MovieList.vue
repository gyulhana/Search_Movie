<template>
  <li v-for="movielist in movielists"
  v-bind:key="movielist.imdbID">
    <div class="movieList"
          @click.prevent="movieModal(movielist.imdbID)">
      <img :src="movielist.Poster" onerror="this.src='https://i.imgur.com/1OpTile.png';" />
      <div class="movieTitle">{{ movielist.Title }},</div>
      <div>{{ movielist.Year }}</div>
    </div>
  </li>
  <div v-if="modal" class="modal-dim" @click.prevent="modalClose">
    <div class="modal">
      <img :src="details.Poster" onerror="this.src='https://i.imgur.com/1OpTile.png';" />
      <div class="text-container">
        <div>Title</div>
        <div>Country</div>
        <div>Language</div>
        <div>Released</div>
        <div>Runtime</div>
        <div>Rated</div>
        <div>Plot</div>
      </div>
      <div class="text-container">
        <div>{{ details.Title }}</div>
        <div>{{ details.Country }}</div>
        <div>{{ details.Language }}</div>
        <div>{{ details.Released }}</div>
        <div>{{ details.Runtime }}</div>
        <div>{{ details.Rated }}</div>
        <div>{{ details.Plot }}</div>
      </div>
    </div>
    </div>
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
    console.log(movieDetaildata)
    return movieDetaildata
   },
   async movieModal(key) {
     this.details = await this.movieDetail(key)
     this.modal = true
     console.log(this.details)

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

.modal-dim {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  z-index: 1000;
  background-color: rgba(0, 0, 0, 0.700);
  .modal {
    background-color: white;
    padding: 10rem;
    border-radius: 15px;
    display: flex;
    flex-direction: row;
    background-color: transparent;
    color: white; 
    gap: 1rem;
    font-size: 1.3rem;
    img {
      width: 20rem;
      height: 25rem;
    }
    .text-container {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      padding: 1.3rem;
      box-sizing: border-box;
    }
  }
}

</style>