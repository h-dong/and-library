<template>
<div class="row search">
  <div class="col-sm-12">
    <form class="search-form">
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
      <div class="alert alert-danger" role="alert">
        No results found.
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
      Book,
    },
    data() {
      return {
        searchText: '',
        library: [],
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
      },
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
            if (cellData.gs$cell.col === '5' && cellData.gs$cell.row !== '1') {
              results.push(tempItem);
              tempItem = {};
            }
          }
        }
        return results;
      },
    },
  };
</script>

<style scoped>
  .search-form {
    margin-top: 20px;
  }
</style>
