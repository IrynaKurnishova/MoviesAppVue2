<template>
  <div class="movie-edit-wrap">
    <header class="movie-edit-header">
      <h6 class="movie-header-title">Edit form</h6>
      <BIconX class="close-icon" @click="emitCloseEditModal"></BIconX>
    </header>
    <div class="movie-edit-content">
      <BContainer fluid>
        <BRow>
          <BCol sm="4">
            <div class="movie-poster-wrap">
              <div class="movie-poster" :style="posterStyle"></div>
            </div>
            <div class="buttons">
              <BButton size="md" block variant="success">Submit</BButton>
              <BButton
                size="md"
                block
                variant="danger"
                @click="emitCloseEditModal"
                >Cancel</BButton
              >
            </div>
          </BCol>
          <BCol sm="8">
            <BContainer fluid>
              <BRow>
                <BCol sm="3" class="movie-title">
                  <label for="input-title">Movie title:</label>
                </BCol>
                <BCol sm="9">
                  <b-form-input
                    v-model="movie.Title"
                    id="input-title"
                    :state="true"
                    :readonly="isReadOnly"
                    trim
                  ></b-form-input>
                </BCol>
              </BRow>
              <BRow>
                <BCol sm="3">
                  <label for="input-plot">Movie Plot:</label>
                </BCol>
                <BCol sm="9">
                  <b-form-input
                    v-model="movie.Plot"
                    id="input-plot"
                    :state="true"
                    :readonly="isReadOnly"
                    trim
                  ></b-form-input>
                </BCol>
              </BRow>
              <BRow>
                <BCol sm="3">
                  <label for="input-year">Movie Year:</label>
                </BCol>
                <BCol sm="9">
                  <b-form-input
                    v-model="movie.Year"
                    id="input-rating"
                    :state="true"
                    :readonly="isReadOnly"
                    trim
                  ></b-form-input>
                </BCol>
              </BRow>
            </BContainer>
          </BCol>
        </BRow>
      </BContainer>
    </div>
  </div>
</template>

<script>
export default {
  name: "MovieEditInfoModal",
  props: {
    movie: {
      type: Object,
      required: true
    }
  },
  data: () => ({
    defaultPosterBg:
      "linear-gradient(45deg, rgb(0, 3, 38) 0%, rgb(82, 35, 117) 100%)",
    isReadOnly: false
  }),
  computed: {
    posterStyle() {
      return {
        "background-image": this.posterBg
      };
    },
    posterBg() {
      return this.movie ? `url(${this.movie.Poster})` : this.defaultPosterBg;
    }
  },
  methods: {
    emitCloseEditModal() {
      this.$emit("closeModal");
    }
  }
};
</script>

<style scoped>
.movie-edit-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: linear-gradient(45deg, rgb(0, 3, 38) 0%, rgb(82, 15, 117) 100%);
  color: white;
}
.movie-header-title {
  margin-bottom: 0;
  line-height: 1.5;
  font-size: 1.25rem;
}
.movie-edit-content {
  padding: 1rem;
  background-color: #fff;
}
.movie-poster-wrap {
  position: relative;
  padding-bottom: 150%;
  border-radius: 5px;
  overflow: hidden;
  transition: all 0.2s ease;
}
.movie-poster {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center center;
}
.close-icon {
  font-size: 24px;
  cursor: pointer;
}
</style>
