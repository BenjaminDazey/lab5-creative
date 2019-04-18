<template>
<div class="home">
  <div>


<div class="image" v-for="photo in photos" v-bind:key="photo._id">
    <router-link :to="{ name: 'photo', params: { id: photo._id }}"><img :src="photo.path" /></router-link>
    <p>{{photo.description}}</p>
    <p class="photoTitle">- {{photo.title}}</p>
<p class="photoDate">
  <span v-if="photo.user.name">{{photo.user.name}}, </span>
  {{formatDate(photo.created)}}
</p>
       </div>
        <form @submit.prevent="sendComment">
            <input v-model="commented" placeholder="edit quote here">
            <p></p>
            <button type="submit" class="pure-button pure-button-secondary">Edit Quote</button>
          </form>
          <form @submit.prevent="deleteQuote">
            <button type="submit" class="pure-button pure-button-secondary">Delete Quote</button>
          </form>
  </div>
</div>
</div>
</template>

<script>
import moment from 'moment';
// @ is an alias to /src
import ImageGallery from '@/components/ImageGallery.vue'

export default {
  name: 'photo',
  components: {
    ImageGallery
  },
  data() {
    return{
      commented: '',
    }
  },
  computed: {
    photos() {
      return this.$store.state.photos;
    },
  },
  methods:{
formatDate(date) {
      if (moment(date).diff(Date.now(), 'days') < 15)
        return moment(date).fromNow();
      else
        return moment(date).format('d MMMM YYYY');
    },
    async sendComment(){
      let idf = this.photos[0]._id;
      await this.$store.dispatch("editQuote", {description: this.commented, id: idf});
    },
    async deleteQuote(){
      let idf = this.photos[0]._id;
      await this.$store.dispatch("deleteOnePhoto", idf);

    }
  },
  async created() {
    await this.$store.dispatch("getOnePhoto", this.$route.params.id);
  
  },
}
</script>

</script>


<style scoped>
.photoTitle {
  margin: 0px;
  font-size: 1.2em;
}

.photoDate {
  margin: 0px;
  font-size: 0.9em;
  font-weight: normal;
}

p {
  margin: 0px;
}

.image {
  margin: 0 0 1.5em;
  display: inline-block;
  width: 100%;
}

.image img {
  max-width: 600px;
  max-height: 600px;
  image-orientation: from-image;
}
</style>