<template>
  <div class="add container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Add Book</h1>
    <form v-on:submit="addBook">
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
        addBook(e){
            if(!this.book.title || !this.book.author || !this.book.isbn){
                this.alert = 'Please fill in all required fields';
                return;
            } 
            if (isNaN(this.book.isbn)) {
                this.alert = 'Please enter a numeric value in the ISBN field';
            }
            else {
                let newBook = {
                    title: this.book.title,
                    author: this.book.author,
                    isbn: this.book.isbn
                    
                }
                
                this.$http.post('http://localhost:8080/api/books', newBook)
                    .then(function(response){
                        this.$router.push({path: '/'});
                        swal("New Book Added!", "", "success");
                        
                    });

                e.preventDefault();
            }
            e.preventDefault();
        }
    },
    components: {
        Alert
    }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>