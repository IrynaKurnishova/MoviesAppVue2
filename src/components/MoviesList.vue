<template>
  <BContainer>
    <h3 class="list-title">{{ listTitle }}</h3>
    <BRow>
      <template v-if="isExist">
        <BCol cols="3" v-for="(movie, key) in list" :key="key">
          <MovieItem
            :movie="movie"
            @mouseover.native="onMouseOver(movie.Poster)"
            @removeItem="onRemoveItem"
            @showModal="
              onShowMovieInfo({ movieID: movie.imdbID, modalType: 'info' })
            "
          />
        </BCol>
      </template>
      <template v-else>
        <div>Empty List</div>
      </template>
    </BRow>
    <BModal
      body-class="movie-modal-body"
      :id="movieInfoModalID"
      size="xl"
      hide-footer
      hide-header
    >
      <MovieInfoModalContent
        :movie="selectedMovie"
        @closeModal="onCloseModal('info')"
        @removeItem="onRemoveItem"
        @openEditModal="onShowMovieInfo"
      />
    </BModal>
    <BModal
      body-class="movie-edit-modal-body"
      :id="movieEditModalID"
      size="xl"
      hide-footer
      hide-header
    >
      <MovieEditInfoModal :movie="selectedMovie" @closeModal="onCloseModal" />
    </BModal>
  </BContainer>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import MovieItem from "@/components/MovieItem.vue";
import MovieInfoModalContent from "./MovieInfoModalContent.vue";
import MovieEditInfoModal from "@/components/MovieEditInfoModal.vue";
export default {
  name: "MoviesList",
  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  data: () => ({
    movieInfoModalID: "movie-info",
    selectedMovieID: "",
    movieEditModalID: "movie-edit"
  }),
  components: {
    MovieItem,
    MovieInfoModalContent,
    MovieEditInfoModal
  },
  computed: {
    ...mapGetters("movies", ["isSearch"]),
    isExist() {
      return Boolean(Object.keys(this.list).length);
    },
    listTitle() {
      return this.isSearch ? "Search result" : "IMDB Top 250";
    },
    selectedMovie() {
      return this.selectedMovieID ? this.list[this.selectedMovieID] : null;
    }
  },
  methods: {
    ...mapActions("movies", ["removeMovie"]),
    ...mapActions(["showNotify"]),
    onMouseOver(poster) {
      this.$emit("changePoster", poster);
    },
    async onRemoveItem({ id, title }) {
      const isConfirmed = await this.$bvModal.msgBoxConfirm(
        `Are you sure delete ${title}?`
      );
      if (isConfirmed) {
        this.removeMovie(id);
        this.showNotify({
          variant: "success",
          msg: "Movie deleted successful",
          title: "Success"
        });
        this.onCloseModal("info");
      }
    },
    onShowMovieInfo({ movieID, modalType }) {
      this.selectedMovieID = movieID;
      const modalID =
        modalType === "info" ? this.movieInfoModalID : this.movieEditModalID;
      this.$bvModal.show(modalID);
    },
    onCloseModal(modalType) {
      const modalID =
        modalType === "info" ? this.movieInfoModalID : this.movieEditModalID;
      this.selectedMovieID = null;
      this.$bvModal.hide(modalID);
    }
  }
};
</script>

<style scoped>
.list-title {
  font-size: 50px;
  margin-bottom: 30px;
  color: #fff;
}
</style>

<style>
.movie-modal-body {
  padding: 0 !important;
}
</style>
