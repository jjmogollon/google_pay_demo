<template>
    <div class="container">
        <div v-if="!response">
                <!-- <h1 class="title has-text-centered">Checkout</h1> -->
            <h2 class="title is-size-3">Checkout</h2>

            <div class="columns">
                <div class="column is-9">
                    <h3 class="title is-size-5">Checkout: Reservation Info</h3>
                    <div class="card">
                        <div class="card-content">
                            <div class="columns">
                                <div class="column">
                                    <figure class="image">
                                        <img :src="room.img_url" alt="Placeholder image">
                                    </figure>
                                </div>
                                <div class="column">
                                    <div class="content">
                                        <h4 class="title is-size-6">{{room.title}}</h4>
                                        <p>{{room.subtitle}}</p>
                                        <p class="has-text-justified">{{room.description}}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="column is-3">
                    <h3 class="title is-size-5">Pay with</h3>
                    <div class="card">
                        <div class="card-content">
                            <div class="content">
                                <p class="price">Total: {{room.price}} €</p>
                                <hr>
                                <div class="checkout">
                                    <div v-show="google_pay_ready" class="google_pay">
                                        <p><b>Pay with Google</b></p>
                                        <div id="google_pay_button"></div>
                                        <hr>
                                    </div>

                                    <form name="contact" method="post" netlify>
                                        <p><b>Pay with Card</b></p>
                                        <input type="hidden">
                                        <div class="field">
                                            <div class="control">
                                                <input class="input" type="text" name="email" placeholder="Email">
                                            </div>
                                        </div>

                                        <div class="field">
                                            <div class="control">
                                                <input class="input" type="text" name="card" placeholder="Card: 4242 4242 4242 4242">
                                            </div>
                                        </div>

                                        <div class="columns">
                                            <div class="column is-6">
                                                <div class="field">
                                                    <div class="control">
                                                        <input class="input" type="text" name="expiration" placeholder="MM/YY">
                                                    </div>
                                                </div>
                                            </div>
                                            <div class="column is-6">
                                                <div class="field">
                                                    <div class="control">
                                                        <input class="input" type="text" name="cvv" placeholder="CVV">
                                                    </div>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="field">
                                            <div class="control">
                                                <button class="button is-primary is-fullwidth">Submit</button>
                                            </div>
                                        </div>
                                    </form>
                                    <div v-show="!google_pay_ready" class="google_pay">
                                        <hr>
                                        <p><b>Pay with Google</b></p>
                                        <div id="google_pay_button_no_ready"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div v-if="response">
            <div class="columns">
                <div class="column  is-three-fifths  is-offset-one-fifth ">
                    <div class="card">
                        <div class="card-content">
                            <div class="content">
                                <h2 class="title has-text-centered has-text-info">Payment Done</h2>
                                <p>
                                    <b>description:</b> {{response.description}}
                                </p>
                                <p>
                                    <b>Masked card:</b> {{response.payload.pan}}
                                </p>
                                <p>
                                    <b>Transaction ID:</b> {{response.payload.transaction_id}}
                                </p>
                                <p>
                                    <b>Order:</b> {{response.payload.order}}
                                </p>
                                <p>
                                    <b>Response code:</b> {{response.payload.code}}
                                </p>
                            </div>
                        </div>
                        </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
import Vue from 'vue';

export default {

  computed: {
      room(){
          return this.rooms[this.$route.params.id]
      }
  },

  data() {
    return {
      rooms: [{
        id: 0,
        title: 'Single room',
        subtitle: 'Single room with bathroom included',
        img_url: 'https://s-ec.bstatic.com/images/hotel/max1024x768/181/181544002.jpg',
        description: 'Madrid House is located in the cosmopolitan district of Madrid in Chueca. It offers elegant rooms with air conditioning and free internet access.',
        price_text: '45€ por noche',
        price: 45
      }, {
        id: 1,
        title: 'Habitación doble',
        subtitle: 'Habitación doble con baño incluido',
        img_url: 'https://s-ec.bstatic.com/images/hotel/max1024x768/181/181544050.jpg',
        description: 'El Madrid House se encuentra en el cosmopolita barrio madrileño de Chueca. Dispone de habitaciones elegantes con aire acondicionado y conexión gratuita a internet.',
        price_text: '60€ por noche',
        price: 60
      }, {
        id: 2,
        title: 'Single room with balcony',
        subtitle: 'Habitación doble con baño incluido',
        img_url: 'https://t-ec.bstatic.com/images/hotel/max1024x768/181/181544026.jpg',
        description: 'El Madrid House se encuentra en el cosmopolita barrio madrileño de Chueca. Dispone de habitaciones elegantes con aire acondicionado y conexión gratuita a internet.',
        price_text: '55€ por noche',
        price: 55
      }],

    response: null,

      google_pay_ready: false,
      google_pay_button: null,

      google_pay: {
        tokenizationSpecification: {
            type: 'PAYMENT_GATEWAY',
            parameters: {
                'gateway': 'sipay',
                'gatewayMerchantId': 'sipay-test-team'
            }
        },

        baseCardPaymentMethod: {
            type: 'CARD',
            parameters: {
                allowedAuthMethods: ["PAN_ONLY", "CRYPTOGRAM_3DS"],
                allowedCardNetworks: ['AMEX', 'DISCOVER', 'JCB', 'MASTERCARD', 'VISA']
            }
        },

        baseRequest: {
            apiVersion: 2,
            apiVersionMinor: 0
        },
        allowedPaymentMethods: ['CARD', 'TOKENIZED_CARD'],
      },
    }
  },

  methods: {
      getGooglePaymentsClient() {
            return (new google.payments.api.PaymentsClient({environment: 'TEST'}));
      },

      getGoogleTransactionInfo() {
        return {
            currencyCode: 'EUR',
            totalPriceStatus: 'FINAL',
            // set to cart total
            // totalPrice: '1.00',
            totalPrice: this.room.price.toString() + '.00'

        }
      },


      getGooglePaymentDataRequest() {
        const paymentDataRequest = Object.assign({}, this.google_pay.baseRequest);
        const cardPaymentMethod = Object.assign({}, this.google_pay.baseCardPaymentMethod,{
            tokenizationSpecification: this.google_pay.tokenizationSpecification
        });

        paymentDataRequest.allowedPaymentMethods = [cardPaymentMethod];
        paymentDataRequest.transactionInfo = this.getGoogleTransactionInfo();
        paymentDataRequest.merchantInfo = {
            // @todo a merchant ID is available for a production environment after approval by Google
            // See {@link https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist|Integration checklist}
            // merchantId: '01234567890123456789',
            // merchantName: 'Example Merchant'
        };
        return paymentDataRequest;
      },

      processPayment(paymentData) {
          var $this = this;
        //   $.ajax({
        //     url: gpay_url + "/store",
        //     method: 'POST',
        //     contentType: 'application/json',
        //     data: JSON.stringify(paymentData)
        // })
        // console.log(paymentData)
        this.$axios.$post('https://develop.sipay.es/dmos/gpay/', paymentData, {
            // baseURL: 'https://develop.sipay.es/dmos/api/v1/gpay/',
            timeout: 3000,
            // withCredentials: true,
            // transformRequest: [(data) => JSON.stringify(paymentData)],
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json',
            }
        })
        .then(function(res){
            return $this.$axios.$post('https://develop.sipay.es/dmos/gpay/sale', {token: res.data.payload.token}, {
                timeout: 20000,
                headers: {
                    'Accept': 'application/json',
                    'Content-Type': 'application/json',
                }
            })
        })
        .then(function(res){
            var to_log = JSON.parse(JSON.stringify(res))
            console.log(to_log.res)
            Vue.set($this, 'response', res.res)
        })
        .catch(function(err){
            console.log('err')
            console.log(err)
        })
        console.log('called axios')
      },

      onGooglePaymentButtonClicked() {
        const $this = this;
        const paymentDataRequest = this.getGooglePaymentDataRequest();
        paymentDataRequest.transactionInfo = this.getGoogleTransactionInfo();

        const paymentsClient = this.getGooglePaymentsClient();
        paymentsClient.loadPaymentData(paymentDataRequest)
            .then(function(paymentData) {
                // handle the response
                console.log(paymentData)
                console.log('processing payment')
                $this.processPayment(paymentData)
                console.log('processed payment')
            })
            .catch(function(err) {
                // show error in developer console for debugging
                console.error(err);
            });
      },

      prefetchGooglePaymentData(paymentsClient) {
          var $this = this
          const button =
            paymentsClient.createButton({onClick: () => $this.onGooglePaymentButtonClicked()});
            var id = $this.google_pay_ready ? 'google_pay_button' : 'google_pay_button_no_ready'
            document.getElementById(id).appendChild(button);
            Vue.set($this, 'google_pay_button', button);
      },

      getGooglePayClient() {
        const $this = this;
        const paymentsClient = this.getGooglePaymentsClient();
        paymentsClient.isReadyToPay({allowedPaymentMethods: $this.google_pay.allowedPaymentMethods})
        .then(function(response) {
            if (response.result) {
                Vue.set($this, 'google_pay_ready', true);
            }
            $this.prefetchGooglePaymentData(paymentsClient);
        })
        .catch(function(err) {
            console.error('err');
            console.error(err);
        });
    }

  },

  mounted() {
      this.getGooglePayClient()
  }
}
</script>


<style lang="sass">
.price
  font-weight: bold
  color: #f85a5a
  font-size: 24px
</style>

