<template>
    <v-layout column justify-center>
        <v-flex xs12 sm6 offset-sm3>
            <v-btn @click="goBack" class="ma-0 mb-3 back-btn" depressed color="orange">Back to search</v-btn>
            <v-card v-if="result">
                <v-card-media :src="result.featured_image" height="200px">
                </v-card-media>
                <v-card-title primary-title>
                    <div >
                        <h3 class="mb-0" color="black">
                            {{ result.name }}
                        </h3>
                        <p class="resto-text">{{ result.cuisines }}</p>
                        <div>{{ result.location.address }}</div>
                        <star-rating
                                :showRating="false"
                                v-model="rating"
                                v-bind:increment="0.5"
                                v-bind:star-size="20"
                        ></star-rating>
                    </div>
                </v-card-title>
            </v-card>
        </v-flex>
    </v-layout>
</template>

<script>

    import StarRating from 'vue-star-rating'
    import {config} from "../config";

    export default {
        name: "RestoView",
        components: {
            StarRating
        },
        props: ['resto'],
        data () {
            return {
                result: '',
                rating: 0
            }
        },
        mounted () {
            const self = this;
            console.log(this.resto);
            self.getResto();
        },

        methods: {
            getResto() {
                this.$http.get(config.API_ENDPOINT + `/restaurant?res_id=` + this.resto.id, { headers: {
                    'Accept': 'application/json',
                    'user-key': config.CLIENT_KEY
                }}).then((response) => {
                    this.result = response.data;
                    this.rating = parseInt(response.data.user_rating.votes);
                })
            },

            goBack() {
                window.history.length > 1 ? this.$router.go(-1) : this.$router.push('/');
            }
        }
    }
</script>

<style scoped>
    .back-btn {
        color: white;
    }
</style>