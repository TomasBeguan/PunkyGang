<template>
  <v-container class="container_cards">
    <div
    v-for="punky in punkys" :key="punky.DocID"  
    class="card_punky"
    >
    <v-card
    class="card_inside border_radius_a"
    max-width="250"
    >

      <v-img
        height="250"
        width="250"
        src="https://davinci.gallery/uploads/artwork/Qmb3PWjkVb5YAbjJjdbPyLJ96TMSkT3MeGJZaDTW8AYrCf"
        class="border_radius_a"
      ></v-img>

      <h1
      class="text-center"
      >
      #129
      </h1>

      <h2 class="text-center">
        Lander {{ punky.mensaje }}
      </h2>


    </v-card>
    </div>
  </v-container>
</template>

<script>
  export default {
    name: 'Main',

    data: () => ({
          punkys: [
          { mensaje: 'Foo' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
          { mensaje: 'Bar' },
        ]
    }),

    methods: {
      
    },

  }
</script>

<style lang="sass">
.container_cards
    display: flex
    flex-wrap: wrap
    justify-content: center
    background-color: #DF57FF
.card_punky
    max-width: 300px
    margin: 10px
    
.card_inside
    padding: 10px


.border_radius_a
    border-radius:15px !important
</style>
