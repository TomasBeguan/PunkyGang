<template>

  <v-container class="container_cards" id="app">

        <!-- NAVIGATION DRAWER -->
        <v-navigation-drawer
          v-model="drawer"
          app
          
        >
        <v-card>
          <v-card-title>
            Filters
          </v-card-title>
        </v-card>
        <v-expansion-panels>
          <v-expansion-panel
            v-for="value in attributes"
            :key="value.DocID"  
          >
              <v-expansion-panel-header>
                {{value}}
              </v-expansion-panel-header>
              <v-expansion-panel-content>
                <v-checkbox
                    v-for="attribute in attributes"
                    :key="attribute.DocID"
                    v-model="selected"
                    :label=attribute
                    :value=attribute
                >
                </v-checkbox>
              </v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-navigation-drawer>
        <!-- NAVIGATION DRAWER -->

        <!-- APP BAR -->
            <v-app-bar app>
            
          <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

          <v-toolbar-title>PUNKY GANG</v-toolbar-title>

          <div class="text-center">
            <v-menu offset-y>
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  color="primary"
                  dark
                  v-bind="attrs"
                  v-on="on"
                >
                  Rarity: High to Low
                  <v-icon
                    right
                    dark
                  >
                    mdi-chevron-down
                  </v-icon>
                </v-btn>
              </template>
              <v-list>
                <v-list-item
                  v-for="(item, index) in items"
                  :key="index"
                  link
                >
                  <v-list-item-title>{{ item.title }}</v-list-item-title>
                </v-list-item>
              </v-list>
            </v-menu>
          </div>


          <v-btn
          rounded
          color="primary"
          dark
          target="_blank"
          href="https://davinci.gallery/artist/punkygang"
          >
            Davinci
          </v-btn>


        </v-app-bar>
        <!-- NAVIGATION DRAWER -->
    
        <div class="buscador_div">
            <v-text-field
                v-model="search"
                label="Search your Punky"
                solo
                clearable
                prepend-inner-icon="mdi-magnify"
                class=""
            ></v-text-field>
        </div>
    
    <div
    v-for="item in filteredList" :key="item.DocID"  
    class="card_punky"
    >
    <v-card
    class="card_inside border_radius_a"
    max-width="300"
    v-bind:href="item.url" 
    target="_blank"
    >

      <v-img
        height="250"
        width="250"
        :src=item.img
        class="border_radius_a"
      ></v-img>

      <h1
      class="text-center"
      >
      #{{item.id}}
      </h1>

      <h2 class="text-center">
        {{ item.nombre }}
      </h2>


    </v-card>
    </div>
  </v-container>
</template>

<script>

import datos from "../assets/json/punkys.json";

  export default {
    name: 'Main',

    props: {
    msg: String
    }, 

    data: () => ({
        drawer: null,
        search: '',
        attributes:{
            Skin: 'Skin',
            Hair: 'Hair',
            HairColor: 'Hair Color',
            Eyes: 'Eyes',
            RightEar: 'Right Ear',
            LeftEar: 'Left Ear',
            Mouth: 'Mouth',
            Background: 'Background',
            Accessory: 'Accessory',
            Extra: 'Extra',
        },
        checks: [
            {Skin:['red', 'white',]},
            {Hair:['Punky','Bum','Knife']}
        ]
        
    }),

    methods: {
      
    },

    computed: {
    items() {
      return datos.map((item) => {
        return item;
      })
    },
    filteredList2() {
      return this.items.filter(item => {
        return item.nombre.toLowerCase().includes(this.search.toLowerCase())
      })
    },
    filteredList() {
        const value= this.search.charAt(0).toUpperCase() + this.search.slice(1);
        return this.items.filter(function(item){
          return item.nombre.indexOf(value) > -1 ||
                 item.id.indexOf(value) > -1 ||
                 item.id.indexOf(value) > -1
        })
    }
  }

  }
</script>

<style lang="sass">
.v-main__wrap
    background-color: #DF57FF
.container_cards
    display: flex
    flex-wrap: wrap
    justify-content: center
.card_punky
    max-width: 300px
    margin: 10px
    cursor: pointer
.card_inside
    padding: 10px

.buscador_div
    width:100%


.border_radius_a
    border-radius:15px !important

.v-input--selection-controls 
    margin-top: 0px !important
    padding-top: 0px !important

</style>
