<template>
  <div class="home">
    <div
      class="home__main"
      :class="{ home__main__success: isSuccess && artists.length !== 0 }"
    >
      <SearchBar :entity="entity" :onSearch="onSearch" :loading="isLoading" />
      <span v-if="isLoading" class="mt-4">LOADING...</span>
      <span v-if="isError && !isLoading"
        >Oops, something went wrong. Try again later...</span
      >
      <List
        v-if="!isLoading && isSuccess"
        :elements="artists"
        :onClick="toggleModal"
      />
      <Modal v-if="showModal" :onClose="toggleModal" />
    </div>
  </div>
</template>

<script>
import { SEARCH_ARTIST } from "@/store/actions";
import List from "@/components/List";
import Modal from "@/components/Modal";
import SearchBar from "@/components/SearchBar";
import { SEARCH_OPTIONS } from "@/constants/search";

export default {
  name: "Home",
  components: {
    List,
    Modal,
    SearchBar
  },
  data: () => ({
    entity: SEARCH_OPTIONS.ALBUM,
    showModal: false
  }),
  computed: {
    artists() {
      return this.$store.getters.artists;
    },
    isLoading() {
      return this.$store.getters.isLoading;
    },
    isError() {
      return this.$store.getters.isError;
    },
    isSuccess() {
      return this.$store.getters.isSuccess;
    }
  },
  methods: {
    onSearch(artist) {
      this.$store.dispatch(SEARCH_ARTIST, { artist });
    },
    toggleModal() {
      this.showModal = !this.showModal;
    }
  }
};
</script>

<style lang="scss">
@import "@/styles/constants";

.home {
  background: $pampas;
  display: flex;
  flex: 1;

  &__main {
    margin-left: auto;
    margin-right: auto;
    margin-top: 30vh;
    width: 70rem;
    transition: 1s;

    &__success {
      margin-top: 10vh;
    }
  }
}
</style>
