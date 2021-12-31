<template>
  <li v-for="movielist in movielists"
  v-bind:key="movielist.imdbID">
    <div class="movieList"
          v-on:click="movieDetail">
      <img :src="movielist.Poster" onerror="this.src='https://i.imgur.com/1OpTile.png';" />
      <div class="movieTitle">{{ movielist.Title }},</div>
      {{ movielist.Year }}
    </div>
  </li>
</template>

<script>
export default {
 props: {
   movielists: Object
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
    width: 100%;
    border-radius: 15px;
    text-align: center;
  }
}
.movieTitle {
  width: 100%;
  overflow:hidden;
      text-overflow:ellipsis;
      white-space:nowrap;
  font-weight: 600;
}

</style>