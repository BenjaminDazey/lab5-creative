<template>
<div class="home">
<p>Here you can see quotes made by other users!</p>
  <image-gallery :photos="photos" />
</div>
<footer>
Github Repository: https://github.com/BenjaminDazey/lab5-creative
</footer>
</template>

<script>
// @ is an alias to /src
import ImageGallery from '@/components/ImageGallery.vue'

export default {
  name: 'home',
  components: {
    ImageGallery
  },
  computed: {
    photos() {
      return this.$store.state.photos;
    }
  },
  async created() {
    await this.$store.dispatch("getAllPhotos");
  },
}
</script>

<style>
p{
  color: #335580;
padding-left: 100px;
padding-right: 100px;
font-size: 2.2em;
}
</style>
