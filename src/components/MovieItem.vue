<template>
  <div class="movie-item mb-5">
    <div class="movie-item-poster" :style="posterBg"></div>
    <div class="movie-info-wrap">
      <div class="movie-item-info">
        <h3 class="movie-title">{{ movie.Title }}</h3>
        <span class="movie-year">{{ movie.Year }}</span>
      </div>
      <div class="movie-item-controls row no-gutters">
        <div class="col pr-2">
          <BButton size="md" block variant="outline-light" @click="showInfoModalEvent">Info</BButton>
        </div>
        <div class="col pl-2">
          <BButton size="md" block variant="outline-light" @click="emitRemoveEvent">Remove</BButton>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'MovieItem',
  props: {
    movie: {
      type: Object,
      required: true
    }
  },
  computed: {
    posterBg() {
      return {
        'background-image': `url(${this.movie.Poster})`
      }
    }
  },
  methods: {
    emitRemoveEvent() {
      this.$emit('removeItem', { id: this.movie.imdbID, title: this.movie.Title });
    },
    showInfoModalEvent() {
      this.$emit('showModal', this.movie.imdbID);
    }
  }
}
</script>


<style scoped>
.movie-item {
  position: relative;
  cursor: pointer;
  border-radius: 5px;
  overflow: hidden;
  transition: all .2s ease;
  height: 400px;
  min-width: 300px;
}

.movie-item-poster {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background: no-repeat center;
  background-size: cover;
  z-index: -1;
}

.movie-item:hover {
  box-shadow: 0 5px 30px rgba(0, 0, 0, 0.7);
  transform: scale(1.02);
}
.movie-info-wrap{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 20px 10px;
  height: 100%;
  opacity: 0;
  transition: all 0.2s ease;
}
.movie-item:hover .movie-info-wrap {
  opacity: 1;
  background-color: rgba(0,0,0,0.7);
}
.movie-title {
  font-size: 20px;
  color: white;
}
.movie-year {
  font-size: 14px;
  color: white;
}

</style>