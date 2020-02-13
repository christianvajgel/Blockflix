<template>
    <div id="app" class="container">
        <h1>Welcome to the {{ title }}!</h1>

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

                    <b-button href="#" variant="primary">Rent!</b-button>
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
                    {id: 1, title: 'Movie 1', description: 'Description of Movie #1', price: 10, image: 'https://i.picsum.photos/id/866/200/300.jpg'},
                    {id: 2, title: 'Movie 2', description: 'Description of Movie #2', price: 20, image: 'https://i.picsum.photos/id/1004/200/300.jpg'},
                    {id: 3, title: 'Movie 3', description: 'Description of Movie #3', price: 30, image: 'https://i.picsum.photos/id/1011/200/300.jpg'},
                    {id: 4, title: 'Movie 4', description: 'Description of Movie #4', price: 40, image: 'https://i.picsum.photos/id/1025/200/300.jpg'},
                    {id: 5, title: 'Movie 5', description: 'Description of Movie #5', price: 50, image: 'https://i.picsum.photos/id/1035/200/300.jpg'},
                    {id: 5, title: 'Movie 6', description: 'Description of Movie #6', price: 60, image: 'https://i.picsum.photos/id/239/200/300.jpg'}
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

</style>