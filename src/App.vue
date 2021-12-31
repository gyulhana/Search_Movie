<template>
  <div class="app__body">
    <div>
      <a href="/">🍊</a>
    </div>
    <div class="search"></div>
    <Search />
  </div>
</template>

<script>
import Search from '~/components/Search'
export default {
  components: {
    Search
  }
}
</script>

<style lang="scss" scoped>
.app__body {
  padding: 2.5rem;
  box-sizing: border-box;
  height: 100%;
  min-height: 800px;
  background-color: #ffd00a;
    background-image: radial-gradient(at 47% 33%, hsl(22.22, 100%, 75%) 0, transparent 59%), radial-gradient(at 82% 65%, hsl(60.00, 100%, 76%) 0, transparent 55%);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}

a {
  text-decoration: none;
  color: black;
  font-size: 5rem;
}

</style>