<template>
  <div class="book-holder">
    <div v-if="editBookHolder" v-on:click="editBookHolder = !editBookHolder">
      <a class="edit-book" role="button">
        <i class="glyphicon glyphicon-pencil"></i>
      </a>
      <span><strong>{{location}}</strong> has this book</span>
    </div>
    <form class="form-inline" v-else>
      <input type="text" class="form-control" placeholder="Location or person">
      <a class="btn btn-success" role="button">
        <i class="glyphicon glyphicon-ok"></i>
      </a>
      <a class="btn btn-default" role="button" v-on:click="editBookHolder = !editBookHolder">
        <i class="glyphicon glyphicon-remove"></i>
      </a>
    </form>
  </div>
</template>

<script>
  export default {
    name: 'bookLocation',
    props: [
      'location'
    ],
    data() {
      return {
        editBookHolder: true
      };
    }
  };
</script>

<style scoped>
  .book-holder {
    position: relative;
    cursor: pointer;
    font-size: 1.1em;
    float: right;
  }

  .book-holder:hover .edit-book {
    visibility: visible;
  }

  .edit-book {
    visibility: hidden;
    color: black;
  }

  .book-holder form {
    top: -9px;
    position: relative;
  }
</style>
