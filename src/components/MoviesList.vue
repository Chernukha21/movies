<template>
  <div>
    <BContainer class="container">
      <h3 class="list-title">{{ listTitle }}</h3>
      <BRow>
        <template v-if="isExist">
          <BCol cols="3" v-for="(movie, key) in list" :key="key" class="table-item">
            <MovieItem :movie="movie"
                       @mouseover.native="onMouseOver(movie.Poster)"
                       @removeItem="onRemoveItem"
                       @showModal="onShowMovieInfo"
                       >
            </MovieItem>
          </BCol>
        </template>
        <template v-else>
          <div>Empty</div>
        </template>
      </BRow>
      <BModal body-class="movie-modal-body p-0"  :id="movieInfoModalID" size="xl" hide-footer hide-header>
        <MovieInfoModalContent :movie="selectedMovie" @closeModal="onCloseModal"/>
      </BModal>
    </BContainer>
  </div>
</template>


<script>
import { mapActions, mapGetters } from 'vuex';
import MovieItem from "./MovieItem";
import MovieInfoModalContent from "./MovieInfoModalContent";
export default {
  name: 'MoviesList',
  components: {
    MovieItem,
    MovieInfoModalContent
  },
  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  data: () => ({
    movieInfoModalID: 'movie-info',
    selectedMovieID: ''
  }),
  computed: {
    ...mapGetters('movies', ['isSearch']),
    isExist(){
      return Boolean(Object.keys(this.list).length);
    },
    listTitle(){
      return this.isSearch ? 'Search result:' : 'IMDB top 250';
    },
    selectedMovie() {
      return this.selectedMovieID ? this.list[this.selectedMovieID] : null;
    }
  },
  methods: {
    ...mapActions('movies',['removeMovie']),
    ...mapActions(['showNotify']),
    onMouseOver(poster) {
      this.$emit('changePoster', poster);
    },
    async onRemoveItem({ id, title }) {
      const isConfirmed = await this.$bvModal.msgBoxConfirm(`Are you sure to remove this ${title}?`);
      if(isConfirmed){
        this.removeMovie(id);
        this.showNotify( {
          msg: 'Movie deleted',
          variant: 'success',
          title: 'Success'
        });
      }
    },
    onShowMovieInfo(id){
      console.log(id);
      this.selectedMovieID = id;
      this.$bvModal.show(this.movieInfoModalID);
    },
    onCloseModal() {
      this.$bvModal.hide(this.movieInfoModalID);
      this.selectedMovieID = null;
    }
  }
}
</script>

<style>
.movie-modal-body {
  padding: 0 !important;
}

</style>

<style scoped>

  .table-item{
    min-width: 300px;
    margin-right: 10px;
  }
  .list-title{
    font-size: 50px;
    margin-bottom: 30px;
    color: white;
  }
</style>

