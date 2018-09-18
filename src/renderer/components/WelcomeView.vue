<template>
  <v-layout row wrap justify-center id="wrapper">
    <v-flex xs12 md12 class="text-xs-center centered">
      <h2>Enter a place to search for a restaurant.</h2>
    </v-flex>
    <v-flex xs10>
      <v-form v-model="valid" ref="form">
        <v-container>
          <v-layout row wrap>
            <v-flex xs12 md12>
              <v-text-field
                      placeholder="Enter place"
                      v-model="query"
                      ></v-text-field>
              <v-btn
                      @click="searchResto"
                      :disabled="!valid"
                      class="ma-0 search-btn"
                      color="light-green lighten-1">Search</v-btn>
            </v-flex>
          </v-layout>
        </v-container>
      </v-form>
    </v-flex>
    <v-flex xs10 class="mt-3">
      <ResultsView :restaurants="this.results" />
    </v-flex>
  </v-layout>
</template>

<script>
  import ResultsView from './ResultsView';
  import {config} from "../config";

  export default {
    name: 'welcome',
    data() {
        return {
            query: '',
            valid: true,
            results: []
        }
    },
    components: { ResultsView },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },

      searchResto() {
          const self = this;

          self.$http.get(config.API_ENDPOINT + `/search?q=` + self.query, { headers: {
              'Accept': 'application/json',
               'user-key': config.CLIENT_KEY
           }}).then((response) => {
               console.log(response.data);
              self.results = response.data;
          })
      }
    }
  }
</script>

<style scoped>
  .centered
  {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .search-btn {
    color: white;
  }
</style>
