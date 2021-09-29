<template>
  <v-app id="inspire">
    <v-main>
      <Main/>
    </v-main>
  </v-app>
</template>



<script>
import Main from './components/Main';

export default {
  name: 'App',

  components: {
    Main,
  },

  data: () => ({
    drawer: null,
    items: [
        { title: 'Rarity: High to Low' },
        { title: 'Rarity: Low to High' },
        { title: 'Number: High to Low' },
        { title: 'Number: Low to High' },
      ],
  }),
};
</script>
