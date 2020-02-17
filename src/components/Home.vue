<template>
    <div id="app" class="container">
        <h1>Welcome to the {{ title }}!</h1>
        <div>
            <div>
                <b-button class="mb-2" variant="dark" @click="$bvToast.show('cart-toast')">
                    <b-icon icon="credit-card"/>
                </b-button>

                <b-toast id="cart-toast" title="Summary:" no-auto-hide>
                    <p v-if="cartQuantity === 0">You don't have movies in your cart. Rent one!</p>
                    <p v-else-if="cartQuantity === 1">You have {{ cartQuantity }} movie in your cart.</p>
                    <p v-else>You have {{ cartQuantity }} movies in your cart.</p>
                </b-toast>
            </div>
        </div>

        <h3 v-if="hours >= 9 && hours < 17" id="open">OPEN</h3>
        <h3 v-else-if="hours >= 17 && hours < 18" id="closing">CLOSING...</h3>
        <h3 v-else id="closed">CLOSED</h3>

        <ul>
            <li v-for="movie in movies" v-bind:key="movie">{{ movie }}</li>
        </ul>

        <b-card
                title="Movie 1"
                img-src="https://picsum.photos/600/300/?image=25"
                img-alt="Image"
                img-top
                tag="article"
                style="max-width: 20rem;"
                class="mb-2"
        >
            <b-card-text>
                Description of Movie 1 and we have a longer phrase because we need to test our page. You should rent this movie.
            </b-card-text>

            <b-button href="#" variant="primary">Rent!</b-button>
        </b-card>

        <div class="row">
            <div class="col-sm-4" v-bind:key="movie.id" v-for="movie in movies">
                <b-card
                        :title="movie.title"
                        :img-src="movie.image"
                        img-alt="Movie Image"
                        img-top
                        tag="article"
                        style="max-width: 20rem;"
                        class="mb-2"
                >
                    <b-card-text>
                        {{ movie.description }} <br> {{ movie.price | formatPrice("U$") }}
                    </b-card-text>

                    <b-button href="#"
                              v-on:click="addToCart(movie)"
                              v-if="validateMovieAdditionToCart(movie)"
                              variant="success">Rent!</b-button>
                    <b-button
                              v-on:click="addToCart(movie)"
                              v-else-if="movie.availableQuantity === 1"
                              variant="outline-warning">Limited quantity!</b-button>
                    <b-button
                              v-else
                              variant="outline-danger disabled">Out of stock!</b-button>
                </b-card>

            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data: function() {
            return {
                title: "Blockflix",
                hours: new Date().getHours(),
                movies: [
                        {
                            id: 1,
                            title: 'Movie 1',
                            description: 'Description of Movie #1',
                            price: 10,
                            image: 'https://i.picsum.photos/id/866/200/300.jpg',
                            availableQuantity: 5
                        },
                        {
                            id: 2,
                            title: 'Movie 2',
                            description: 'Description of Movie #2',
                            price: 20,
                            image: 'https://i.picsum.photos/id/1004/200/300.jpg',
                            availableQuantity: 5
                        },
                        {
                            id: 3,
                            title: 'Movie 3',
                            description: 'Description of Movie #3',
                            price: 30,
                            image: 'https://i.picsum.photos/id/1011/200/300.jpg',
                            availableQuantity: 5
                        },
                        {
                            id: 4,
                            title: 'Movie 4',
                            description: 'Description of Movie #4',
                            price: 40,
                            image: 'https://i.picsum.photos/id/1025/200/300.jpg',
                            availableQuantity: 5
                        },
                        {
                            id: 5,
                            title: 'Movie 5',
                            description: 'Description of Movie #5',
                            price: 50,
                            image: 'https://i.picsum.photos/id/1035/200/300.jpg',
                            availableQuantity: 5
                        },
                        {
                            id: 6,
                            title: 'Movie 6',
                            description: 'Description of Movie #6',
                            price: 60,
                            image: 'https://i.picsum.photos/id/239/200/300.jpg',
                            availableQuantity: 5
                        }
                    ],
                cart:[
                ]
            }
        },
        filters: {
            formatPrice: function (price, symbol) {
                if (!parseInt(price)){
                    return "";
                }
                let formattedPrice = (price.toFixed(2).replace(",","."));
                return symbol + " " + formattedPrice;
            }
        },
        methods: {
            addToCart: function (movie) {
                this.cart.push(movie.id);
            },
            movieQuantityInCart: function (movie) {
                let quantity = 0;
                for (let i = 0; i < this.cart.length; i++) {
                    if (movie.id === this.cart[i]){
                        quantity++;
                    }
                }
                return quantity;
            },
            validateMovieAdditionToCart: function (movie) {
                // if (movie.availableQuantity > this.movieQuantityInCart(movie)){
                //     return 1;
                // } else if(movie.availableQuantity > this.movieQuantityInCart(movie)
                // && movie.availableQuantity === 1){
                //     return 0;
                // } else {
                //     return -1;
                // }
                return movie.availableQuantity > this.movieQuantityInCart(movie);
            }
        },
        computed: {
            cartQuantity: function () {
                return this.cart.length;
            }
        }
    };
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    #open {
        color: blue;
    }

    #closing {
        color: orange;
    }

    #closed {
        color: red;
    }

    .b-toast * {
        font-style: italic !important;
    }

</style>