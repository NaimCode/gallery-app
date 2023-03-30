<script>
import axios from "axios";
export default {
  name: "ImageView",

  components: {},
  data() {
    return {
      image: {},
      isLoading: false,
    };
  },
  created() {
    this.getImage();
  },
  methods: {
    getImage() {
      this.isLoading = true;
      axios
        .get(
          `https://api.unsplash.com/photos/${this.$route.params.id}?client_id=lebTpI4Osa9WxNrZiPtmv2bQaeFaV7r4fQgoCQ6e-88`
        )
        .then((response) => {
          this.image = response.data;
          console.log("response.data :>> ", response.data);
          this.isLoading = false;
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<template>
  <div class="image">
    <button
      class="fixed top-3 left-4 bg-black/50 text-xs text-white py-1 px-3 z-50 rounded-md"
    >
      <router-link to="/">Back</router-link>
    </button>
    <div v-if="isLoading" class="flex justify-center">
      <div
        class="loader ease-linear rounded-full border-4 border-t-4 border-gray-200 h-12 w-12 mb-4"
      ></div>
    </div>
    <div
      v-else
      class="h-screen w-screen bg-center bg-repeat bg-cover backdrop-brightness-125 filter flex flex-col justify-center gap-6 p-[300px]"
      :style="{ 'background-image': 'url(' + image.urls.full + ')' }"
    >
      <h1 class="text-5xl text-white">
        {{ image.user.name }}
      </h1>
      <h2 class="text-2xl text-white">
        {{ image.description }}
      </h2>

      <div class="flex gap-4 text-xs">
        <a
          :href="image.links.html"
          target="_blank"
          class="bg-white text-black px-4 py-2 rounded-lg"
        >
          View the Image on Unsplash
        </a>
        <a
          :href="image.user.links.html"
          target="_blank"
          class="bg-white text-black px-4 py-2 rounded-lg"
        >
          View Author on Unsplash
        </a>
      </div>
    </div>
  </div>
</template>
