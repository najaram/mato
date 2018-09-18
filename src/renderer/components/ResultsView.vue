<template>
    <v-layout row wrap justify-center>
        <v-flex class="pa-2" xs12 sm6 v-if="restaurants.results_found > 1" v-for="restaurant in restaurants.restaurants">
            <v-card>
                <v-card-media
                        v-if="restaurant.restaurant.featured_image"
                        :src="restaurant.restaurant.featured_image"
                        class="resto-image mb-0"
                        height="200"
                        alt=""></v-card-media>
                <v-card-media
                        src="https://via.placeholder.com/350x150"></v-card-media>
                <v-card-title primary-title>
                    <div>
                        <h3 class="mb-0" color="black">
                            {{ restaurant.restaurant.name }}
                        </h3>
                        <p class="resto-text">{{ restaurant.restaurant.cuisines }}</p>
                        <div>{{ restaurant.restaurant.location.address }}</div>
                    </div>
                </v-card-title>
                <v-card-actions>
                    <router-link
                            :to="{ name: 'resto-view', params: {resto: restaurant.restaurant} }"
                            color="orange"
                            class="pa-2 resto-link"
                            :restaurant="restaurant.restaurant"
                    >visit</router-link>
                </v-card-actions>
            </v-card>
        </v-flex>
    </v-layout>
</template>

<script>
    export default {
        name: "ResultsView",
        props: ['restaurants'],
        mounted() {
            console.log(this.restaurants, 'results view');
        }
    }
</script>

<style scoped>
    .resto-text {
        color: orange;
    }
    
    .resto-link {
        color: orange;
        text-decoration: none;
        text-transform: capitalize;
    }
</style>