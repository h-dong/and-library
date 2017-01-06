<template>
<div class="row search">
  <div class="col-sm-12">
    <form>
      <div class="form-group">
        <input type="text" class="form-control" v-model="searchText" placeholder="Type to search... e.g. The Lean Startup">
      </div>
    </form>
  </div>
  <div class="col-sm-12">
    <div v-if="searchResults.length > 0">
      <book v-for="item in searchResults" track-by="$index" :book-data="item"></book>
    </div>
    <div v-else>
      <div class="spinner">
        <div class="rect1"></div>
        <div class="rect2"></div>
        <div class="rect3"></div>
        <div class="rect4"></div>
        <div class="rect5"></div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
  import Book from './Book';

  export default {
    name: 'search',
    components: {
      Book
    },
    data() {
      return {
        searchText: '',
        library: []
      };
    },
    computed: {
      searchResults() {
        const lowercaseSearch = this.searchText.toLowerCase();
        return this.library.filter((data) => {
          const matchAuthor = (data.BookAuthor.toLowerCase().indexOf(lowercaseSearch) > -1);
          const matchTitle = (data.BookTitle.toLowerCase().indexOf(lowercaseSearch) > -1);
          const matchSummary = (data.BookSummary.toLowerCase().indexOf(lowercaseSearch) > -1);
          return matchAuthor || matchTitle || matchSummary;
        });
      }
    },
    mounted() {
      this.getBookData();
    },
    methods: {
      getBookData() {
        const documentId = '1ufwR0gRtCDZ5jyo9mn2OFTonGt3P0p4YBoouh6BWn1A';
        const sheetId = 'o6x90qy';
        const googleSheetApi = `https://spreadsheets.google.com/feeds/cells/${documentId}/${sheetId}/public/values?alt=json`;
        this.$http.get(googleSheetApi)
          .then((response) => {
            this.library = this.convertDataArray(response.body);
            console.log(this.library);
          });
      },
      convertDataArray(json) {
        const tableData = json.feed.entry;
        const results = [];

        if (tableData) {
          const headings = [];
          let tempItem = {};
          for (const cellData of tableData) {
            if (cellData.gs$cell.row === '1') {
              headings.push(cellData.content.$t.replace(/ /g, ''));
            } else {
              const currentHeading = headings[cellData.gs$cell.col - 1];
              tempItem[currentHeading] = cellData.content.$t;
            }
            if (cellData.gs$cell.col === '6' && cellData.gs$cell.row !== '1') {
              tempItem.BookRow = cellData.gs$cell.row;
              results.push(tempItem);
              tempItem = {};
            }
          }
        }
        return results;
      }
    }
  };
</script>

<style>
  .spinner {
    margin: 100px auto;
    width: 50px;
    height: 40px;
    text-align: center;
    font-size: 10px;
  }

  .spinner>div {
    background-color: #bfbfbf;
    height: 100%;
    width: 6px;
    display: inline-block;
    -webkit-animation: sk-stretchdelay 1.2s infinite ease-in-out;
    animation: sk-stretchdelay 1.2s infinite ease-in-out;
  }

  .spinner .rect2 {
    -webkit-animation-delay: -1.1s;
    animation-delay: -1.1s;
  }

  .spinner .rect3 {
    -webkit-animation-delay: -1.0s;
    animation-delay: -1.0s;
  }

  .spinner .rect4 {
    -webkit-animation-delay: -0.9s;
    animation-delay: -0.9s;
  }

  .spinner .rect5 {
    -webkit-animation-delay: -0.8s;
    animation-delay: -0.8s;
  }

  @-webkit-keyframes sk-stretchdelay {
    0%,
    40%,
    100% {
      -webkit-transform: scaleY(0.4)
    }
    20% {
      -webkit-transform: scaleY(1.0)
    }
  }

  @keyframes sk-stretchdelay {
    0%,
    40%,
    100% {
      transform: scaleY(0.4);
      -webkit-transform: scaleY(0.4);
    }
    20% {
      transform: scaleY(1.0);
      -webkit-transform: scaleY(1.0);
    }
  }
</style>
