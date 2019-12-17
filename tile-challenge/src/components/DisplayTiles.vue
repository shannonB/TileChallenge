<template>
  <div>
    <div class="header">
      <h1>BUNGALOW</h1>
    </div>
    <div class="header" style="text-align:right">
      <h3>Seattle Listings</h3>
    </div>
    <hr />
    <div id="tiles">
      <div class="tile" v-for="property in properties" :key="property.id">
        <div class="house-img">
          <img :src="getImageTag(property)" />
        </div>
        <div class="availability">
          <span>
            {{ property.available_room_count }} of {{ property.total_room_count }} rooms available
            {{ whenAvailable(property.earliest_available_date) }}
          </span>
        </div>
        <div class="headline">
          <span>{{ property.headline }}</span>
        </div>
        <div class="prices">
          <span>{{ getPriceRange(property.room_prices) }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';

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
  methods: {
    getImageTag(property) {
      if (property.images.length === 0) {
        return 'https://s3-us-west-2.amazonaws.com/fieldstone-stage-public-media/markets/seattle-xlg3x.png';
      }
      return property.images[0].md_url;
    },
    whenAvailable(availableDate) {
      const availableMoment = moment(availableDate, 'YYYY-MM-DD');
      if (moment().diff(availableMoment) > 0) {
        return 'today';
      }
      return availableMoment.fromNow();
    },
    getPriceRange(prices) {
      const max = Math.max(...prices);
      const min = Math.min(...prices);
      if (max === min) {
        return `$${max.toString()}`;
      }
      return `$${min.toString()}-${max.toString()}`;
    },
  },
};
</script>

<style scoped>
.header {
  display: inline-block;
  width: 50%;
}
.tile {
  display: inline-block;
  width:300px;
  height:300px;
  margin: 10px 20px;
  border-bottom:1px solid grey;
}
.house-img {
  width: 275px;
  height:200px;
}
.house-img img{
  max-width:100%;
  max-height: 100%;
}
.availability span, .prices span {
  font-size:13px;
}
</style>
