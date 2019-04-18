<template>
<div>
  <div v-if="user">
    <div class="header">
      <div>
        <h1>Welcome, {{user.name}}.</h1>
      </div>
      <div>
          <a @click="toggleUpload"><i class="far fa-image"></i></a>
          <a href="#" @click="logout"><i class="fas fa-sign-out-alt"></i></a>
      
      </div>
    </div>
    <p>Have you ever wanted to attribute Ron Swanson Quotes to historical figures or friends? Now is your chance! Click upload to see a new Ron Swanson quote and enter your friend's name and a picture!</p>
    <h1>Take a look at your current quotes below! Click one to edit the quote or delete it!</h1>
<escape-event @escape="escape"></escape-event>
<uploader :show="show" @escape="escape" @upload-finished="uploadFinished" />
<image-gallery :photos="photos" />
  </div>
  <div v-else>
    <router-link to="/register" class="pure-button">Register</router-link> or
    <router-link to="/login" class="pure-button">Login</router-link>
    <p>Have you ever wanted to attribute Ron Swanson Quotes to historical figures or friends? Now is your chance! Register for an account or login to start saving your Ron Swanson quotes!</p>

  </div>
</div>
</template>

<script>
import EscapeEvent from '@/components/EscapeEvent.vue'
import Uploader from '@/components/Uploader.vue'
import ImageGallery from '@/components/ImageGallery.vue'

export default {
  name: 'mypage',
    components: {
    EscapeEvent,
    Uploader,
    ImageGallery
  },
  data() {
    return {
      show: false,
    }
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
      photos() {
      return this.$store.state.photos;
    }
  },
  async created() {
    this.$store.dispatch("getUser");
    await this.$store.dispatch("getMyPhotos");
  },
  methods: {
    async logout() {
      try {
        this.error = await this.$store.dispatch("logout");
      } catch (error) {
        console.log(error);
      }
    },
      async uploadFinished() {
      this.show = false;
      try {
        this.error = await this.$store.dispatch("getMyPhotos");
      } catch (error) {
        console.log(error);
      }
    },
        escape() {
      this.show = false;
    },
    toggleUpload() {
      this.show = true;
    },
  },
}
</script>

<style scoped>
.header {
  display: flex;
}

p {
color: #335580;
padding-left: 100px;
padding-right: 100px;
font-size: 2.2em;
}

.header a {
  padding-left: 50px;
  color: #222;
  font-size: 2em;
}

.header svg {
  margin-top: 12px;
}
</style>

