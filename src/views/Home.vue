<template>
  <q-layout>
    <q-page-container>
      <q-page style="padding:25px;">
        <div style="margin-bottom: 25px;">
          <p>
            Hello Nicolas,
          </p>
          <p>
            Where do you want to go?
          </p>
          <div>
            <q-search class="shadow-8" style="padding: 20px; border-radius: 5px;" id="location" icon="fas fa-map-marker-alt" v-model="search" placeholder="Start typing a city name">
              <q-autocomplete @search="searchCity" @selected="selected" />
            </q-search>
          </div>
        </div>
        <q-card inline class="q-sm" style="padding: 0px;">
          <q-card-media>
            <img src="../assets/ec55923c51b108886098b20ff9968edc.jpg">

            <q-card-title slot="overlay">
              Title
            </q-card-title>
          </q-card-media>
        </q-card>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<style>
</style>

<script>
import algoliasearch from "algoliasearch";

function filterResults(results) {
  return results.map(x => {
      return {
        label: x.locale_names[0],
        sublabel: x.country,
        id: x.objectID
      };
  });
}

export default {
  name: 'PageHome',
  data: () => {
    return {
      search: '',
      places: null
    }
  },
  mounted: function() {
    this.$data.places = algoliasearch.initPlaces('pl74WS5X4SHD', '88a5697f7f3b61a09839ba18174b6538');
  },
  methods: {
    searchCity(terms, done) {
      this.$data.places.search({
        query: terms,
        hitsPerPage: 5,
        type: 'city',
        language: 'en'
      }, function(err, res) {
        done(filterResults(res.hits));
      });
    },
    selected(item) {
      console.log(item);
    }
  }
}
</script>
