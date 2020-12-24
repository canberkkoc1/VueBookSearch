<template>
  <div id="app">
    <main>
      <div class="search-box w-25">        
        <b-input-group>
          <b-form-input v-model="book_query" @keyup.enter ="fetchBooks"></b-form-input>
          <b-input-group-append>
            <b-button variant="info" @click="fetchBooks" class="outline:none"><b-icon icon="Search" ></b-icon></b-button>
          </b-input-group-append>
        </b-input-group>
      </div>
              <div class="cards">
          <b-card v-for="(item,index ) in books.items" :key="index">
            <b-card-img :src="item.volumeInfo.imageLinks.thumbnail" alt="Image" ></b-card-img>
            <b-card-title>{{item.volumeInfo.title}}</b-card-title>
            <b-card-text v-if="item.volumeInfo.description == ' '">
               Description not define
            </b-card-text>
            <b-card-text v-else>
              {{item.volumeInfo.description | sizeChar(100)}}
            </b-card-text>

            <b-button :href="item.volumeInfo.canonicalVolumeLink" variant="primary" target="_blank">Go Link</b-button>
          </b-card>
        </div>
    </main>
    
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

export default {
  name: 'App',
  data(){
    return{
      book_query:"",
      api_base:"https://www.googleapis.com/books/v1/",
      api_key:"AIzaSyCgA_tmwaj7OsYqrf8UuJPHFulcfSgPnhU",
      books:{},
      //
    }
  },
  methods:{
    // get all book data
    fetchBooks(){
      fetch(`${this.api_base}volumes?q=${this.book_query}&key=${this.api_key}`)
      .then(res =>{
        return res.json()
      }).then(this.ShowResults)
    },
    ShowResults(results){
      this.books = results;
    }
  },
  filters:{
    //character restriction
    sizeChar: function(value, size){
        if(!value) return '';
        value = value.toString();

        if(value.length <= size){
          return value
        }
        return value.substr(0, size) + '...';
    }
  }
}
</script>

<style>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.search-box{
  margin: 0 auto;
}
.cards{
  margin:3.5rem ;
  grid-gap: 5rem;
  display: grid;
  grid-template-columns: repeat(5 , 1fr);
}
.b-button{
 outline: none; box-shadow: none;

}
</style>
