<template>
    <div class="container">
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
                                <form name="contact" netlify>
                                    <p><b>Pay with Card</b></p>
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
                                    <hr>
                                    <p><b>Pay with Google</b></p>

                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
export default {

  computed: {
      room(){
        //   console.log(this.$route.params.id);
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
      }]
    }
  }
}
</script>
