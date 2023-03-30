<script>
// @ is an alias to /src

import axios from "axios";
export default {
  name: "SearchView",
  components: {},
  data() {
    return {
      photos: [],
      searchText: "",
      isLoading: false,
    };
  },
  created() {
    this.loadPhotos();
  },
  methods: {
    toImage(id) {
      this.$router.push(`/image/${id}`);
    },
    async loadPhotos() {
      try {
        this.isLoading = true;
        const response = await axios.get(
          `https://api.unsplash.com/search/photos?query=${this.searchText}&per_page=30&client_id=lebTpI4Osa9WxNrZiPtmv2bQaeFaV7r4fQgoCQ6e-88`
        );
        this.photos = [...response.data.results];
      } catch (error) {
        console.error(error);
      } finally {
        this.isLoading = false;
      }
    },
  },
};
</script>

<template>
  <div class="home relative">
    <div
      class="flex justify-center p-10 mx-auto w-full sticky top-0 left-0 z-50 backdrop-blur-md"
    >
      <input
        v-model="searchText"
        @keyup.enter="loadPhotos"
        class="bg-white/90 font-semibold text-xs py-2 px-4 rounded-md z-50 backdrop-blur-lg w-[500px]"
        type="text"
        placeholder="Search"
      />
    </div>
    <div class="grid grid-cols-3 gap-4">
      <div
        v-for="photo in photos"
        :key="photo.id"
        class="bg-gray-200 rounded-lg relative h-auto overflow-hidden"
        @click="toImage(photo.id)"
      >
        <img
          :src="photo.urls.regular"
          class="object-cover w-full transform h-full hover:rotate-6 duration-500 hover:scale-150 transition-all hover:contrast-150"
        />
      </div>
    </div>
    <div v-if="isLoading" class="flex justify-center">
      <div
        class="loader ease-linear rounded-full border-4 border-t-4 border-gray-200 h-12 w-12 mb-4"
      ></div>
    </div>
  </div>
</template>
