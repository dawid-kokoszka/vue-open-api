<template>
  <section class="home">
    <div class="container">
      <h1><strong>Photo Albums</strong></h1>
      <div class="heading">
        <h6>Search & Filter:</h6>
        <input type="text" placeholder="Find by title" v-model="searchTitle">
        <select v-model="selectedUser" v-if="usersList != null">
          <option value=''>All Author</option>
          <option
            v-for="user in usersList"
            :key="user.id"
            :value="user.id"
          >
            {{ user.name }}
          </option>
        </select>
      </div>
      <div class="body">
        <div class="albums-wrapper" v-if="albumsList != null">
          <div class="album-item"
            v-for="album in getSortedAlbums"
            :key="album.id"
            @click="$router.push({
              name: 'albumDetails', 
              params:{id:album.id},
              query:{albumTitle: album.title}
            })"
          >
            <h4>{{ album.title }}</h4>
            <img src="@/assets/album-icon.svg" alt="album" width="60">
            <h6>Author: <strong>{{ getAuthorOfAlbum(album.userId) }}</strong></h6>
            <div class="hover">
              <img src="@/assets/right-arrow.svg" alt="album" width="35">
            </div>
          </div>
          <h3 v-if="getSortedAlbums.length == 0">No results...</h3>
        </div>
        <h6 v-else>Wait for data...</h6>
      </div>
    </div>
  </section>
</template>

<script>
import _ from "lodash"
export default {
  name: "home",
   data () { 
    return { 
      albumsList: null,
      usersList: null,
      selectedUser: '',
      searchTitle: ''
    } 
  },
  created(){
    this.getAlbums();
    this.getUsers();
  },
  computed:{
    getSortedAlbums(){
      return this.albumsList.filter(item =>{
        return item.title.toLowerCase().includes(this.searchTitle.toLowerCase()) && item.userId.toString().includes(this.selectedUser)
      })
    }
  },
  methods:{
    getUsers(){
      this.$http.get('https://jsonplaceholder.typicode.com/users').then((result) => {
        console.log(result)
        this.usersList = result.body
      }).catch((err) => {
        console.log(err)
        this.usersList = []
      });
    },
    getAlbums(){
      this.$http.get('https://jsonplaceholder.typicode.com/albums').then((result) => {
        console.log(result)
        this.albumsList = result.body
      }).catch((err) => {
        console.log(err)
        this.albumsList = []
      });
    },
    getAuthorOfAlbum(id){
      let author = _.find(this.usersList, ['id', id])
      if(author != undefined){
        return author.name
      }else{
        return 'No author'
      }
    }
  }
}
</script>
