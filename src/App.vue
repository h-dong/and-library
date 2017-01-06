<template>
  <div class="container">
    <site-header></site-header>
    <search></search>
  </div>
</template>

<script>
import SiteHeader from './components/SiteHeader';
import Search from './components/Search';

export default {
  name: 'app',
  components: {
    SiteHeader,
    Search
  }
};
</script>
