<template>
  <div class="search">
    <form @submit.prevent="searchMovies">
      <input class="keyword"
            placeholder="보고 싶은 영화를 검색하세요."
            ref="keyword"
            autocomplete="off" />
    </form>
    <div class="resultNumber" v-if="searched">
      {{ totalResults }}개의 결과를 발견했습니다.
    </div>
    <div class="resultNumber" v-if="error">
      세 글자 이상의 검색어를 입력해주세요.
    </div>
    <div class="resultNumber" v-if="noResult">
      해당하는 검색 결과가 없습니다.
    </div>
    <ul v-if="searched">
      <MovieList :movielists="this.movieLists" />
    </ul>
    <div class="paging" v-if="searched">
      <button @click="pageDown">◀</button>
      {{ page }} 페이지
      <button @click="pageUp">▶</button>
    </div>
  </div>
</template>

<script>
import MovieList from './MovieList'
export default {
  components: {
    MovieList
  },
  data() {
    return {
      inputKeyword: '',
      page: 1,
      totalResults: 0,
      searched: false,
      error: false,
      noResult: false,
      movieLists: []
    }
  },
  methods: {
    stateInit() {
      this.error = false
      this.noResult = false
      this.searched = false
      this.page = 1
      this.inputKeyword = ''
    },
    async searching() {
      const inputKeyword = this.$refs.keyword.value.trim()
      if (inputKeyword.length > 2) {
        const searchResult = await fetch(`http://www.omdbapi.com/?apikey=7035c60c&s=${inputKeyword}&page=${this.page}`,{
          method: 'GET',
        }).then(res => res.json())
        if (!searchResult.Error) {
          this.totalResults = searchResult.totalResults
          this.movieLists = searchResult.Search
          this.searched = true
        } else if (searchResult.Error == "Movie not found!") {
          this.noResult = true
        }
      } else {
        this.error = true
      }
    },
    async searchMovies() {
      this.stateInit()
      this.searching()
    },
    async pageUp() {
      if (this.totalResults / 10 > this.page) {
        this.page += 1
        this.searching()
      }
    },
    async pageDown() {
      if (this.page > 1) {
        this.page -= 1
        this.searching()
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
  gap: 0.5rem;
}
.keyword {
  border: none;
  border-radius: 20px;
  width: 30rem;
  height: 3rem;
  text-align: center;
  box-sizing: border-box;
  &:focus {
    outline: none;
  }
}
.resultNumber {
  margin: 1rem 0;
  text-align: center;

}
button {
  border: none;
  background-color: transparent;
  cursor: pointer;
  font-size: 1rem;
}

ul {
  display: grid;
  grid-template-columns: repeat(4, minmax(100px, 250px));
  grid-auto-rows: 1fr;
  grid-gap: 1rem;
}
</style>