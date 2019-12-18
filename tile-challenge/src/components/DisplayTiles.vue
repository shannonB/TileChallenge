<template>
  <div>
    <div class="header">
      <h1>BUNGALOW</h1>
    </div>
    <div class="header" style="text-align:right">
      <h3>Seattle Listings</h3>
    </div>
    <hr />
    <div>
      <h4>{{ count }} RESULTS</h4>
    </div>
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
          <span><b>{{ getPriceRange(property.room_prices) }}</b></span>
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
    return { properties: [], count: 0 };
  },
  mounted() {
    axios({ method: 'GET', url: 'https://stage-fieldstone.bungalow.com/api/v1/listings/properties/?market__slug=seattle' }).then((result) => {
      this.properties = result.data.results;
      this.count = result.data.count;
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
      return `$${min.toString()} - ${max.toString()}`;
    },
  },
};
</script>

<style scoped>
#tiles {
  text-align: center;
}
.header {
  display: inline-block;
  width: 50%;
}
.tile {
  display: inline-block;
  width: 300px;
  height: 300px;
  margin: 10px 20px;
  border-bottom: 1px solid grey;
  text-align: left;
}
.house-img {
  height: 200px;
  text-align: center;
  width: 100%;
}
.house-img img{
  max-width: 100%;
  height: 175px;
}
.headline {
  height: 2em;
  margin-top: 8px;
}
.availability span, .prices span {
  font-size: 13px;
}
.availability {
  display: inline;
  background-color: aliceblue;
  padding: 3px;
  border: 1px solid gray;
  border-radius: 5px;
}
.prices {
  margin-top: 5px;
}
@media only screen and (max-width: 350px) {
  h1 {
    font-size: 22px;
  }
  h3 {
    font-size: 16px;
  }
}
</style>
