<template>
  <section class="container">
    <!-- <div class="columns">
      <div class="column">
        <h1 class="title has-text-centered">DEMO: Book a reservation</h1>
      </div>
    </div> -->
    <div class="columns">
      <div class="column is-4"  v-for="room in rooms" :key="room.id">
        <div class="card">
          <div class="card-image">
            <figure class="image is-4by3">
              <img :src="room.img_url" alt="Placeholder image">
            </figure>
          </div>
          <div class="card-content">
            <div class="media">
              <div class="media-content">
                <p class="title is-4">{{room.title}}</p>
                <p class="subtitle is-6">{{room.subtitle}}</p>
              </div>
            </div>

            <div class="content">
              <p class="description">{{room.description}}</p>
              <div class="columns">
                <div class="column">
                  <nuxt-link :to="{name: 'checkout-id', params: {id: room.id}}" class="button is-primary">Reservar</nuxt-link>
                </div>
                <div class="column">
                  <span class="price">{{room.price_text}}</span> <br>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
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

<style lang='sass'>
.price
  font-weight: bold
  color: #f85a5a
  font-size: 24px

.description
  margin-top: 5px
  font-size: 14pxç

.title
  margin: 30px auto
</style>
