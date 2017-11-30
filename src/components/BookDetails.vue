<template>
  <div class="details container">
    <router-link to="/">Back</router-link>
    <h1 class="page-header">{{book.title}}
      <span class="pull-right">
        <router-link class="btn btn-primary" v-bind:to="'/edit/'+book.id">Edit</router-link>
        <button class="btn btn-danger" v-on:click="deleteBook(book.id)">Delete</button>
      </span>
    </h1>
    <ul class="list-group">
        <li class="list-group-item"><h2> Author: {{book.author}} </h2></li>
        <li class="list-group-item"><h2>ISBN: {{book.isbn}} </h2></li> 
    </ul>

    
  </div>
</template>

<script>
export default {
  name: 'bookdetails',
  data () {
    return {

      book: ''
      
    }
  },

  methods:{

    fetchBook(id){
      this.$http.get('http://localhost:8080/api/books/'+id)
        .then(function(response){
          
          this.book = (response.body); 
        });
    },

    deleteBook(id){
          this.$http.delete('http://localhost:8080/api/books/'+id)
          .then(function(response){
            this.$router.push({path: '/'});
            swal("Book Deleted!", "", "success");
          });
      },
  },

  created: function(){
    this.fetchBook(this.$route.params.id);
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
