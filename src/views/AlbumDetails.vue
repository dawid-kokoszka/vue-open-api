<template>
  <div class="album-details">
    <div class="container">
      <h1>Photos of <strong>{{ this.$route.query.albumTitle }}</strong></h1>
      <div class="body">
        <div class="photos-wrapper" v-if="photosList != null">
      
          <div class="photo-item"
            v-for="photo in photosList"
            :key="photo.id"
          >
          <img @click="showPhoto(photo.url)" :src="photo.thumbnailUrl" :alt="photo.title" width="150">
            
          </div>
         
          <h3 v-if="photosList.length == 0">No results...</h3>
        </div>
        <h6 v-else>Wait for data...</h6>
      </div>
    </div>
    <div class="gallery-modal" @click.stop="closeModal" :class="{'open':openModal}">
      
      <div  @click.stop class="inner-wrapper">
        <div @click.stop="closeModal" class="close btn">x</div>
        <img :src="actualURL">
      </div>
    </div>
  </div>
</template>
<script>
import _ from "lodash"
export default {
  name: "albumDetails",
   data () { 
    return { 
      photosList: null,
      actualURL: '',
      openModal: false
    } 
  },
  created(){
    this.getPhotos();
    console.log(this.$route)
  },
  methods:{
    showPhoto(url){
      this.openModal = true;
      this.actualURL = url
    },
    closeModal(){
      this.openModal=false
      this.actualURL=''
    },
    getPhotos(){
      this.$http.get('https://jsonplaceholder.typicode.com/photos?albumId='+this.$route.params.id).then((result) => {
        console.log(result)
        this.photosList = result.body
      }).catch((err) => {
        console.log(err)
        this.photosList = []
      });
    }
  }
}
</script>
