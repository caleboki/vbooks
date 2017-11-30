<template>
  <div class="edit container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Edit Book</h1>
    <form v-on:submit="updateBook">
        <div class="well">
            <h4>Book Info</h4>
            <div class="form-group">
                <label>Title</label>
                <input type="text" class="form-control" placeholder="Book Title" v-model="book.title">
            </div>
            <div class="form-group">
                <label>Author</label>
                <input type="text" class="form-control" placeholder="Author" v-model="book.author">
            </div>
            <div class="form-group">
                <label>ISBN</label>
                <input type="text" class="form-control" placeholder="ISBN" v-model="book.isbn">
            </div>
        </div>

        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
    import Alert from './Alert'
    export default {
    name: 'add',
    data () {
        return {
        book: {},
        alert:''
        }
    },
    methods: {
        fetchBook(id){
            this.$http.get('http://localhost:8080/api/books/'+id)
            .then(function(response){
                this.book= response.body;
            });
        },
        updateBook(e){
            if(!this.book.title || !this.book.author || !this.book.isbn){
                this.alert = 'Please fill in all required fields';
            } else {
                let updBook = {
                    id: this.$route.params.id,
                    title: this.book.title,
                    author: this.book.author,
                    isbn: this.book.isbn
                }
                this.$http.put('http://localhost:8080/api/books/', updBook)
                    .then(function(response){
                        this.$router.push({path: '/'});
                        swal("Book Updated!", "", "success");
                    });
                e.preventDefault();
            }
            e.preventDefault();
        }
    },
    created: function(){
        this.fetchBook(this.$route.params.id);
    },
    components:{
        Alert
    }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>