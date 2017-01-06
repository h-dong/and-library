<template>
  <div class="panel panel-default">
    <div class="panel-heading">
      <h3 class="panel-title">{{bookData.BookTitle}} <span class="text-muted">by {{bookData.BookAuthor}}</span></h3>
    </div>
    <div class="panel-body">
      <div class="row">
        <div class="col-sm-12">
          <img :src="bookData.BookImageLink" :alt="'Book Image for ' + bookData.BookTitle">
          <p>{{bookData.BookSummary}}</p>
        </div>
        <div class="col-sm-12">
          <div class="row">
            <div class="col-sm-6 pull-left">
              <span class="label label-success">Date added {{bookData.Timestamp}}</span>
            </div>
            <div class="col-sm-6 pull-right">
              <book-location :location="bookData.BookLocation"></book-location>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import BookLocation from './BookLocation';

  export default {
    name: 'book',
    components: {
      BookLocation
    },
    props: [
      'bookData'
    ],
    data() {
      return {
        bookHolder: 'hello world'
      };
    }
  };
</script>

<style scoped>
  img {
    width: 200px;
    float: left;
    margin-right: 20px
  }

  p {
    text-align: justify;
    white-space: pre-wrap;
  }
</style>
