<template>
    <b-container id="app" class="container">
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

            <div>
                <b-button class="mb-2" variant="info" @click="showCart">
                    Cart: {{ movieQuantityInCart }} movies.
                </b-button>
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
                class="mb-2">
            <b-card-text>
                Description of Movie 1 and we have a longer phrase because we need to test our page. You should rent this movie.
            </b-card-text>

            <b-button href="#" variant="primary">Rent!</b-button>
        </b-card>

        <b-container class="row" v-if="showMovies">
            <b-container class="col-sm-4" v-bind:key="movie.id" v-for="movie in movies">
                <b-card
                        :title="movie.title"
                        :img-src="movie.image"
                        img-alt="Movie Image"
                        img-top
                        tag="article"
                        style="max-width: 20rem;"
                        class="mb-2">
                    <b-card-text>
                        {{ movie.description }} <br> {{ movie.price | formatPrice("U$") }}
                    </b-card-text>

                    <b-container>
                        <span v-if="movie.ranking === 5.0"><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/></span>
                        <span v-else-if="movie.ranking === 4.5"><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-half/></span>
                        <span v-else-if="movie.ranking === 4.0"><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/><b-icon-star/></span>
                        <span v-else-if="movie.ranking === 3.5"><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-half/><b-icon-star/></span>
                        <span v-else-if="movie.ranking === 3.0"><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-fill/><b-icon-star/><b-icon-star/></span>
                        <span v-else-if="movie.ranking === 2.5"><b-icon-star-fill/><b-icon-star-fill/><b-icon-star-half/><b-icon-star/><b-icon-star/></span>
                        <span v-else-if="movie.ranking === 2.0"><b-icon-star-fill/><b-icon-star-fill/><b-icon-star/><b-icon-star/><b-icon-star/></span>
                        <span v-else-if="movie.ranking === 1.5"><b-icon-star-fill/><b-icon-star-half/><b-icon-star/><b-icon-star/><b-icon-star/></span>
                        <span v-else-if="movie.ranking === 1.0"><b-icon-star-fill/><b-icon-star/><b-icon-star/><b-icon-star/><b-icon-star/></span>
                        <span v-else-if="movie.ranking === 0.5"><b-icon-star-half/><b-icon-star/><b-icon-star/><b-icon-star/><b-icon-star/></span>
                        <span v-else v-for="n in 5" :key="n"><b-icon-star/></span>
                    </b-container>
                    <br><br>

                    <b-button href="#"
                              v-on:click="addToCart(movie)"
                              v-if="movie.availableQuantity > 1"
                              variant="outline-success">Rent! {{ movie.availableQuantity }}</b-button>
                    <b-button
                              v-on:click="addToCart(movie)"
                              v-else-if="movie.availableQuantity === 1"
                              variant="outline-warning" id="btn">Limited quantity! {{ movie.availableQuantity }}</b-button>
                    <b-button
                              v-else
                              variant="outline-danger disabled">Out of stock! {{ movie.availableQuantity }}</b-button>
                </b-card>
            </b-container>
        </b-container>

        <b-container class="row" v-else>
            <b-container>
                <b-card bg-variant="light" style="text-align: left">
                    <b-form-group
                            label-cols-lg="3"
                            label="Checkout"
                            label-size="lg"
                            label-class="font-weight-bold pt-0"
                            class="mb-0">

                        <b-form-group
                                id="field_first_name"
                                label-cols="4"
                                label-cols-lg="2"
                                label="First name"
                                label-for="first_name"
                                :invalid-feedback="first_name_invalidFeedback"
                                :valid-feedback="first_name_validFeedback"
                                :state="first_name_State">
                            <b-form-input
                                    id="first_name"
                                    type="text"
                                    placeholder="Enter your first name"
                                    v-model="order.first_name"
                                    :state="first_name_State"
                                    trim>
                            </b-form-input>
                        </b-form-group>

                        <b-form-group
                                id="field_last_name"
                                label-cols="4"
                                label-cols-lg="2"
                                label="Last name"
                                label-for="last_name"
                                :invalid-feedback="last_name_invalidFeedback"
                                :valid-feedback="last_name_validFeedback"
                                :state="last_name_State">
                            <b-form-input
                                    id="last_name"
                                    type="text"
                                    placeholder="Enter your last name"
                                    v-model="order.last_name"
                                    :state="last_name_State"
                                    trim>
                            </b-form-input>
                        </b-form-group>

                        <b-form-group
                                id="field_street"
                                label-cols="4"
                                label-cols-lg="2"
                                label="Street"
                                label-for="street"
                                :invalid-feedback="street_invalidFeedback"
                                :valid-feedback="street_validFeedback"
                                :state="street_State">
                            <b-form-input
                                    id="street"
                                    type="text"
                                    placeholder="Enter your street"
                                    v-model="order.street"
                                    :state="street_State"
                                    trim>
                            </b-form-input>
                        </b-form-group>

                        <b-form-group
                                id="field_city"
                                label-cols="4"
                                label-cols-lg="2"
                                label="City"
                                label-for="city"
                                :invalid-feedback="city_invalidFeedback"
                                :valid-feedback="city_validFeedback"
                                :state="city_State">
                            <b-form-input
                                    id="city"
                                    type="text"
                                    placeholder="Enter your city"
                                    v-model="order.city"
                                    :state="city_State"
                                    trim>
                            </b-form-input>
                        </b-form-group>

                        <b-form-group
                                id="field_state"
                                label-cols="4"
                                label-cols-lg="2"
                                label="State"
                                label-for="state">
                            <b-form-select v-model="selected" :options="states"></b-form-select>
                        </b-form-group>

                        <b-form-group
                                id="field_cep"
                                label-cols="4"
                                label-cols-lg="2"
                                label="CEP"
                                label-for="cep"
                                :invalid-feedback="cep_invalidFeedback"
                                :valid-feedback="cep_validFeedback"
                                :state="cep_State">
                            <b-form-input
                                    id="cep"
                                    type="number"
                                    placeholder="Enter your CEP"
                                    v-model="order.cep"
                                    :state="cep_State">
                            </b-form-input>
                        </b-form-group>

                        <b-form-group
                                id="field_checkbox"
                                label-cols="4"
                                label-cols-lg="2"
                                label="Pay on delivery?"
                                label-for="checkbox">
                            <b-form-checkbox
                                    id="payOnDelivery"
                                    name="checkbox"
                                    class="form-check-input"
                                    v-bind:true-value="order.yesOnDelivery"
                                    v-bind:false-value="order.noOnDelivery"
                                    v-model="order.payOnDelivery">
                            </b-form-checkbox>
                        </b-form-group>

                        <b-form-group
                                id="field_radio"
                                label-cols="4"
                                label-cols-lg="2"
                                label="Delivery period?"
                                label-for="radio">
                            <b-form-radio-group v-model="value" :options="order.deliveryOptions" :state="radioButton_state" name="radio-validation">
                                <b-form-invalid-feedback :state="radioButton_state">Please select one day period.</b-form-invalid-feedback>
                                <b-form-valid-feedback :state="radioButton_state">Valid option!</b-form-valid-feedback>
                            </b-form-radio-group>
                        </b-form-group>

                    </b-form-group>

                    <b-form-group id="buttonAlignment">
                        <b-button variant="dark" v-on:click="submitForm" >Pay order</b-button>
                    </b-form-group>

                </b-card>
            </b-container>
        </b-container>
    </b-container>
</template>

<script>
    export default {
        name: 'app',
        data: function() {
            return {
                value: null,
                time: new Date().getHours,
                order: {
                    first_name: "",
                    last_name: "",
                    street: "",
                    city: "",
                    state: "",
                    cep: "",
                    payOnDelivery: "No",
                    yesOnDelivery: "Yes",
                    noOnDelivery: "No",
                    delivery: this.value,
                    deliveryOptions: [
                        {text:"Morning", value: "first"},
                        {text:"Afternoon", value: "second"},
                        {text:"Evening", value: "third"},
                    ],
                },
                showMovies: true,
                title: "Blockflix",
                cart:[],
                hours: new Date().getHours(),
                movies: [
                        {
                            id: 1,
                            title: 'Movie 1',
                            description: 'Description of Movie #1',
                            price: 10,
                            image: 'https://i.picsum.photos/id/866/200/300.jpg',
                            availableQuantity: 5,
                            ranking: 5
                        },
                        {
                            id: 2,
                            title: 'Movie 2',
                            description: 'Description of Movie #2',
                            price: 20,
                            image: 'https://i.picsum.photos/id/1004/200/300.jpg',
                            availableQuantity: 5,
                            ranking: 4.5
                        },
                        {
                            id: 3,
                            title: 'Movie 3',
                            description: 'Description of Movie #3',
                            price: 30,
                            image: 'https://i.picsum.photos/id/1011/200/300.jpg',
                            availableQuantity: 5,
                            ranking: 3.0
                        },
                        {
                            id: 4,
                            title: 'Movie 4',
                            description: 'Description of Movie #4',
                            price: 40,
                            image: 'https://i.picsum.photos/id/1025/200/300.jpg',
                            availableQuantity: 5,
                            ranking: 2.5
                        },
                        {
                            id: 5,
                            title: 'Movie 5',
                            description: 'Description of Movie #5',
                            price: 50,
                            image: 'https://i.picsum.photos/id/1035/200/300.jpg',
                            availableQuantity: 5,
                            ranking: 1
                        },
                        {
                            id: 6,
                            title: 'Movie 6',
                            description: 'Description of Movie #6',
                            price: 60,
                            image: 'https://i.picsum.photos/id/239/200/300.jpg',
                            availableQuantity: 5,
                            ranking: 0.5
                        }
                ],
                selected: null,
                states:[
                        {value: null, text:"Please select an option.", disabled: true},
                        {
                            label: 'North',
                            options: [
                                {value: "AM", text:"Amazonas"},
                                {value: "RO", text:"Roraima"},
                                {value: "AP", text:"Amapá"},
                                {value: "PA", text:"Pará"},
                                {value: "TO", text:"Tocantins"},
                                {value: "RO", text:"Rondônia"},
                                {value: "AC", text:"Acre"},
                            ]
                        },
                        {
                            label: 'Northeast',
                            options: [
                                {value: "MA", text:"Maranhão"},
                                {value: "PI", text:"Piauí"},
                                {value: "CE", text:"Ceará"},
                                {value: "RN", text:"Rio Grande do Norte"},
                                {value: "PE", text:"Pernambuco"},
                                {value: "PB", text:"Paraíba"},
                                {value: "SE", text:"Sergipe"},
                                {value: "AL", text:"Alagoas"},
                                {value: "BA", text:"Bahia"},
                            ]
                        },
                        {
                            label: 'Midwest',
                            options: [
                                {value: "MT", text:"Mato Grosso"},
                                {value: "MS", text:"Mato Grosso do Sul"},
                                {value: "GO", text:"Goiás"},
                            ]
                        },
                        {
                            label: 'Southeast',
                            options: [
                                {value: "SP", text:"São Paulo"},
                                {value: "RJ", text:"Rio de Janeiro"},
                                {value: "ES", text:"Espírito Santo"},
                                {value: "MG", text:"Minas Gerais"},
                            ]
                        },
                        {
                            label: 'South',
                            options: [
                                {value: "PR", text:"Paraná"},
                                {value: "RS", text:"Rio Grande do Sul"},
                                {value: "SC", text:"Santa Catarina"},
                            ]
                        },
                        {
                            label: 'Federal District',
                            options: [
                                {value: "DF", text:"Distrito Federal"},
                            ]
                        },
                ],
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
            showCart() {
                this.showMovies = this.showMovies ? false : true;
            },
            addToCart: function (movie) {
                this.cart.push(movie.id);
                movie.availableQuantity--;
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
                // return movie.availableQuantity > this.movieQuantityInCart(movie);
            },
            submitForm() {
                alert("Order created.");
            }
        },
        computed: {
            cartQuantity: function () {
                return this.cart.length;
            },
            first_name_State() {
                return this.order.first_name.length >= 4;
            },
            first_name_invalidFeedback() {
                if (this.order.first_name.length >= 4){
                    return "";
                } else if (this.order.first_name.length > 0){
                    return "Enter a name with at least four characters.";
                } else {
                    return "Enter something...";
                }
            },
            first_name_validFeedback() {
                return this.first_name_State === true ? "Valid first name!" : "";
            },
            last_name_State() {
                return this.order.last_name.length >= 4;
            },
            last_name_invalidFeedback() {
                if (this.order.last_name.length >= 4){
                    return "";
                } else if (this.order.last_name.length > 0){
                    return "Enter a name with at least four characters.";
                } else {
                    return "Enter something...";
                }
            },
            last_name_validFeedback() {
                return this.last_name_State === true ? "Valid last name!" : "";
            },
            street_State() {
                return this.order.street.length >= 5;
            },
            street_invalidFeedback() {
                if (this.order.street.length >= 5){
                    return "";
                } else if (this.order.street.length > 0){
                    return "Enter an street with at least five characters.";
                } else {
                    return "Enter something...";
                }
            },
            street_validFeedback() {
                return this.street_State === true ? "Valid street!" : "";
            },
            city_State() {
                return this.order.city.length >= 6;
            },
            city_invalidFeedback() {
                if (this.order.city.length >= 6){
                    return "";
                } else if (this.order.city.length > 0){
                    return "Enter a city with at least six characters.";
                } else {
                    return "Enter something...";
                }
            },
            city_validFeedback() {
                return this.city_State === true ? "Valid city!" : "";
            },
            cep_State() {
                return this.order.cep.length >= 9;
            },
            cep_invalidFeedback() {
                if (this.order.cep.length >= 9){
                    return "";
                } else if (this.order.cep.length > 0){
                    return "Enter a CEP with at least nine characters.";
                } else {
                    return "Enter something...";
                }
            },
            cep_validFeedback() {
                return this.cep_State === true ? "Valid CEP!" : "";
            },
            radioButton_state() {
                return Boolean(this.value);
            },
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

    #buttonAlignment {
        text-align: right !important;
    }

</style>