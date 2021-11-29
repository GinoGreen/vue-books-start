<template>
  <div>

    <Search 
      @sendSearch="performSearch"
      @reset="performReset"
    />

    <Loader 
      titleLoader="Ricerca in corso..."
      v-if="isLoading"
    />

    <!-- wrapper -->
    <div v-else>
      <h2>Chiave di ricerca: {{ typeToSearch }} - {{ textToSearch}}</h2>
      <table class="table table-striped my-5">
        <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Autore</th>
            <th scope="col">Titolo</th>
            <th scope="col">Anno di pubblicazione</th>
            <th scope="col">Anno ultima edizione</th>
            <th scope="col">ISBN ultima edizione</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(book, index) in books"
            :key="book.key"
          >
            <th scope="row">{{ index + 1 }}</th>
            <td>
              <p v-for="(author, key) in book.author_name"
                :key="key"
              >{{ author }}</p>
            </td>
            <td>{{ book.title }}</td>
            <td>{{ book.first_publish_year }}</td>
            <td>{{ getLastPublishYear(book) }}</td>
            <td>{{ getIsbn(book) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <!-- /wrapper -->

  </div>
</template>

<script>

import axios from 'axios';
import Search from './Search'
import Loader from './Loader.vue';

export default {
  name: 'ListBooks',
  components: {
    Search,
    Loader
  },
  data() {
    return {
      books: [],
      typeToSearch: '',
      textToSearch: '',
      apiURL: 'http://openlibrary.org/search.json?',
      isLoading: false
    }
  },
  methods:{
    getApi(){
      this.isLoading = true;
      // chiamata da rendere dinamica
      axios.get(`${this.apiURL}${this.typeToSearch}=${this.textToSearch}`)
      .then(r =>{

        this.books = r.data.docs;
        this.isLoading = false;
      })
      .catch( e => {
        console.log(e);
      })
    },

    getIsbn(book) {

      if (book.isbn === undefined) return 'NN';
      return book.isbn[book.isbn.length - 1];
    },

    getLastPublishYear(book) {

      if (book.publish_year === undefined) return book.first_publish_year;
      return book.publish_year[book.publish_year.length - 1] ;
    },

    performSearch(text, type) {
      
      this.textToSearch = text;
      this.typeToSearch = type;
      
      this.getApi();
    },
    performReset() {

      this.textToSearch = '';
      this.typeToSearch = '';

      this.books = [];
    }
  }
}
</script>

<style>

</style>