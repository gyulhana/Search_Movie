<template>
  <div class="search">
      <input class="keyword"
            placeholder="보고 싶은 영화를 검색하세요."
            ref="keyword"
            autocomplete="off"
            v-on:keyup.prevent="searchingInput" />
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
      movieLists: [],
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
    async searchingInput() {
      setTimeout(() => {this.inputKeyword = this.$refs.keyword.value.trim()
      if (this.inputKeyword.length > 2) {
        this.error = false
        this.searching()
      } else {
        this.stateInit()
        this.error = true
      }}, 500)
    },
    async searching() {
        const searchResult = await fetch(`https://www.omdbapi.com/?apikey=7035c60c&s=${this.inputKeyword}&page=${this.page}`,{
          method: 'GET',
        }).then(res => res.json())
        if (!searchResult.Error) {
          this.totalResults = searchResult.totalResults
          this.movieLists = searchResult.Search
          this.searched = true
          this.noResult = false
        } else if (searchResult.Error == "Movie not found!") {
          this.stateInit()
          this.noResult = true
        }
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
  margin-top: 1rem;
}

ul {
  display: grid;
  width: 90vw;
  grid-template-columns: repeat(auto-fit, minmax(12rem, 1fr));
  grid-auto-rows: 4fr;
  grid-gap: 1rem;
}
</style>