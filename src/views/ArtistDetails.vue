<template>
  <div class="wrapper" v-if="painting">
    <!--hero-->
    <div class="hero">
      <!--hero image-->
      <picture>
        <source 
          :srcset="painting.images.hero.large"
          media="(min-width: 768px)"
        />
        <img :src="painting.images.hero.small" alt=""/>
      </picture>
      <!--hero header group-->
      <div class="heading-group">
        <h2>{{ painting.name }}</h2>
        <h3>{{ painting.artist.name }}</h3>

        <!--hero artist image-->
        <div class="artist-image">
          <img :src="painting.artist.image" alt=""/>
        </div>
      </div>
      <!--hero button-->
      <BaseButton 
        @click="$refs.modalName.openModal()"
        class="btn btn--hero"
        txt="View Image"
      />
    </div>

    <!--body-->
    <main class="main">
      <!--body year-->
      <h2 class="year">{{ painting.year }}</h2>
      <!--body copy-->
      <p>{{ painting.description }}</p>
      <!--body-link -->
      <a class="btn btn--link" :href="painting.source" target="_blank">
        Go To Source 
      </a>
    </main>
  </div>

  <footer class="footer" v-if="painting">
    <!--progress bar-->
    <div class="progress" :style="fullWidth"></div>
    <!--pagination-->
    <div class="pagination">

    </div>
  </footer>
</template>

<script>
import data from '../assets/shared/data.json';
import BaseButton from "../components/BaseButton.vue";
import Modal from '../components/Modal.vue';
import Pagination from "../components/Pagination.vue";

export default {
  name: "ArtistDetails",
  components: {
    BaseButton,
    Modal,
    Pagination, 
  },
  props: {
    id: {
      type: Number,
      required: true,
    },
    startShow: Boolean,
  },
  data() {
    return {
      width: this.id, 
    };
  },
  watch: {
    startShow() {
      this.startSlideShow();
    },
  },
  computed: {
    painting() {
      return data.paintings.find((painting) => painting.id === this.id);
    },
    fullWidth() {
      return `width:${this.width * 6.666667}%`;
    },
  },
  methods: {
    prev() {
      if (this.id > 1) {
        this.$router.push({
          name: "ArtistDetails",
          params: { id: this.id - 1 },
        });
        this.width = this.id - 1;
      }
    },
    next() {
      if (this.id < 15) {
        this.$router.push({
          name: "ArtistDetails",
          params: { id: this.id + 1},
        });
        this.width = this.id + 1;
      }
    },
    startSlideShow() {
      let t;
      if (this.startShow && !this.t) {
        this.t = setInterval(this.next, 5000);
      } else {
        clearInterval(this.t);
      }
    },
  },
  mounted() {
    this.startSlideShow(); 
  },
};
</script>

<style>

</style>
