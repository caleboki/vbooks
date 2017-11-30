<template>
  <div class="books container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Manage Books</h1>
    <input class="form-control" placeholder="Search" v-model="filterInput">
    <br />
    <table class="table table-striped">
        <thead>
          <tr>
            <th>Title</th>
            <th>Author</th>
            <th>ISBN</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="book in filterBy(books, filterInput)">
            <td>{{book.title}}</td>
            <td>{{book.author}}</td>
            <td>{{book.isbn}}</td>
            <td><router-link class="btn btn-default" v-bind:to="'/books/'+book.id">View</router-link></td>
          </tr>
        </tbody>
    </table>
  </div>
</template>

<script>
  import Alert from './Alert';
  export default {
    name: 'books',
    data () {
      return {
        books: [],
        alert:'',
        filterInput:''
      }
    },
    methods: {
      fetchBooks(){
        this.$http.get('http://localhost:8080/api/books')
          .then(function(response){
            this.books = response.body;
          });
      },
      filterBy(list, value){
        value = value.charAt(0).toUpperCase() + value.slice(1);
        return list.filter(function(book){
          return book.title.indexOf(value) > -1 || book.isbn.indexOf(value) > -1 || book.author.indexOf(value) > -1;
        });
      }
    },
    created: function(){
      if(this.$route.params.alert){
        this.alert = this.$route.params.alert;
      }
      this.fetchBooks();
    },
    updated: function(){
      this.fetchBooks();
    },
    components: {
      Alert
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>