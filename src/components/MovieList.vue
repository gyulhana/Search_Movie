<template>
  <li v-for="movielist in movielists"
  v-bind:key="movielist.imdbID">
    <div class="movieList">
      <img :src="movielist.Poster" onerror="this.src='https://i.imgur.com/1OpTile.png';" />
      {{ movielist.Title }}, {{ movielist.Year }}
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
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  flex-direction: column;
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
    height: 9rem;
    max-width: 90%;
    border-radius: 15px;
  }
}

</style>