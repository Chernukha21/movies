<template>
  <div id="app">
    <Loader/>
    <Notification/>
    <Header/>
    <PosterBg :poster="posterBg" />
    <Movies-list :list="moviesList" @changePoster="onChangePoster"/>
    <MoviesPagination
        :current-page="currentPage"
        :per-page="moviesPerPage"
        :total="moviesLength"
        @pageChanged="onPageChanged"/>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
import MoviesList from "./components/MoviesList";
import PosterBg from "./components/PosterBg";
import MoviesPagination from "./components/MoviesPagination";
import Loader from "./components/Loader";
import Header from "./components/Header";
import Notification from "./components/Notification";
export default {
  name: 'App',
  components: {
    MoviesList,
    PosterBg,
    MoviesPagination,
    Loader,
    Header,
    Notification
  },
  data: () => ({
    posterBg: ''
  }),
  computed: {
    ...mapGetters('movies',['moviesList', 'currentPage', 'moviesPerPage', 'moviesLength'])
  },
  watch: {
    '$route.query': {
      handler: 'onPageQueryChange',
      immediate: true,
      deep: true
    }
  },
  methods: {
    ...mapActions('movies',['changeCurrentPage']),
    onPageQueryChange({ page = 1}){
      this.changeCurrentPage(Number(page));
    },
    onChangePoster(poster) {
      this.posterBg = poster;
    },
    onPageChanged(page) {
      this.$router.push({ query: { page }});
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>
