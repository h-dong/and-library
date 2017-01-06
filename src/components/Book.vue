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
          <span class="label label-success pull-right">Date added {{bookData.Timestamp}}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'search',
    props: [
      'bookData'
    ],
    data() {
      return {};
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
