<template>
  <div class="hello">
    <h2>All information: {{ properties }}</h2>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'DisplayTiles',
  data() {
    return { properties: [] };
  },
  mounted() {
    axios({ method: 'GET', url: 'https://stage-fieldstone.bungalow.com/api/v1/listings/properties/?market__slug=seattle' }).then((result) => {
      this.properties = result.data.results;
    }, (error) => {
      console.error('Error getting property data', error);
    });
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
