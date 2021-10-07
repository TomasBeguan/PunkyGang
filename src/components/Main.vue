<template>
  <v-container class="container_cards" id="app">
    <!-- NAVIGATION DRAWER -->
    <v-navigation-drawer v-model="drawer" app>
      <v-card>
        <v-card-title>
          Filters
        </v-card-title>
      </v-card>
      <v-expansion-panels>
        <v-expansion-panel v-for="(cb, cbindex) in checkboxes" :key="cb.title">
          <v-expansion-panel-header>{{ cb.title }}</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-checkbox
              v-model="cb.selected"
              v-for="(chkbox, index) in cb.checkboxList"
              :label="chkbox.label + ` (${chkbox.number})`"
              :value="chkbox.value"
              @change="checkboxFilter(cbindex)"
              :key="index"
            >
            </v-checkbox>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-navigation-drawer>
    <!-- NAVIGATION DRAWER -->

    <!-- APP BAR -->
    <v-app-bar app class="navbar">
      <v-app-bar-nav-icon @click="drawer = !drawer" dark></v-app-bar-nav-icon>

      <v-toolbar-title class="hidden-md-and-down logoPunky" dark
        >PUNKY GANG</v-toolbar-title
      >

      <div class="text-center">
        <v-menu offset-y class="btnRarity" transition="slide-x-transition">
          <template v-slot:activator="{ on, attrs }">
            <v-btn v-bind="attrs" v-on="on" class="btnRarity">
              {{ org }}
              <v-icon right dark>
                mdi-chevron-down
              </v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item link
              ><v-list-item-title v-on:click="cambiarOrg('rhtl')"
                >Rarity: High to Low</v-list-item-title
              ></v-list-item
            >
            <v-list-item link
              ><v-list-item-title v-on:click="cambiarOrg('nhtl')"
                >Number: High to Low</v-list-item-title
              ></v-list-item
            >
          </v-list>
        </v-menu>
      </div>

      <v-btn
        dark
        rounded
        target="_blank"
        href="https://davinci.gallery/artist/punkygang"
        class="btnDavinci"
      >
        Davinci
      </v-btn>
    </v-app-bar>
    <!-- NAVIGATION DRAWER -->

    <div class="buscador_div">
      <v-text-field
        color="deep-purple"
        v-model="search"
        label="Search your Punky"
        solo
        clearable
        prepend-inner-icon="mdi-magnify"
        class="textField"
        rounded
      ></v-text-field>
    </div>

    <div v-for="item in filteredItems" v-bind:key="item.id" class="card_punky">
      <v-card
        class="card_inside border_radius_a"
        max-width="300"
        v-bind:href="item.url"
        target="_blank"
      >
        <v-img
          height="250"
          width="250"
          :src="item.img"
          class="border_radius_a"
        ></v-img>

        <h1 class="text-center cardID">#{{ item.id }}</h1>

        <h2 class="text-center cardName">
          {{ item.nombre }}
        </h2>
      </v-card>
    </div>
  </v-container>
</template>

<script>
import datos from "../assets/json/punkyGang.json";
export default {
  name: "Main",

  props: {
    msg: String,
  },

  data: () => ({
    uniqueId: null,
    org: "Number: High to Low",
    orgID: "item.DocID",
    drawer: null,
    search: "",
    attributes: {
      Skin: "Skin",
    },
    filteredItems: [],
    originalItems: [],
    checkboxes: [],
    titulos:[
      "Hair",
      "Skin"
    ],
  }),
  watch: {
    search(value) {
      value = value.toLowerCase();
      this.filteredItems = this.originalItems;
      this.filteredItems = this.filteredItems.filter((item) => {
        if (
          item.nombre.toLowerCase().includes(value) ||
          item.id.toLowerCase().includes(value)
        ) {
          return item;
        }
      });
    },
  },
  methods: {
    cambiarOrg(value) {
      if (value == "rhtl") {
        this.org = "Rarity: High to Low";
        this.filteredItems = this.filteredItems.sort(function(a, b) {
          return a.rarity - b.rarity;
        });
      }
      if (value == "nhtl") {
        this.org = "Number: High to Low";
        this.filteredItems = this.filteredItems.sort(function(a, b) {
          return b.id - a.id;
        });
      }
    },
    checkboxFilter() {
      const isEmpty = this.checkboxes.every((item) => {
        return !item.selected.length;
      });

      if (isEmpty) {
        return (this.filteredItems = [...this.originalItems]);
      }
      this.filteredItems = this.originalItems.filter((item) => {
        for (let index = 0; index < this.checkboxes.length; index++) {
          if (
            (this.checkboxes[index].selected.includes(
              item.skin.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "skin") ||
            (this.checkboxes[index].selected.includes(
              item.hair.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "hair") ||
            (this.checkboxes[index].selected.includes(
              item.hairColor.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "hairColor") ||
            (this.checkboxes[index].selected.includes(
              item.eyes.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "eyes") ||
            (this.checkboxes[index].selected.includes(
              item.background.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "background") ||
            (this.checkboxes[index].selected.includes(
              item.accessory.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "accessory") ||
            (this.checkboxes[index].selected.includes(
              item.extra.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "extra")
          ) {
            return item;
          }
        }
      });
    },
    createCheckBox(checkboxCategory) {
      let newarray = this.originalItems;

      let categoryArray = newarray.map((item) => {
        if (Object.keys(item).includes(checkboxCategory)) {
          return {
            [checkboxCategory]: item[checkboxCategory],
          };
        }
      });

      let countData = [];
      categoryArray.reduce((acc, curr) => {
        const str = JSON.stringify(curr);

        acc[str] = (acc[str] || 0) + 1;

        let foundIndex = countData.findIndex(
          (item) =>
            item.categoryName == checkboxCategory &&
            item.value == curr[checkboxCategory]
        );

        if (foundIndex >= 0) {
          countData.splice(foundIndex, 1, {
            categoryName: checkboxCategory,
            value: curr[checkboxCategory],
            number: countData[foundIndex].number + 1,
          });
        } else if (foundIndex < 0) {
          countData.push({
            categoryName: checkboxCategory,
            value: curr[checkboxCategory],
            number: 1,
          });
        }

        return acc;
      }, []);

      let CheckboxData = countData.map((item) => {
        return {
          label: item.value,
          value: item.value.toLowerCase().trim(),
          number: item.number,
        };
      });

      this.checkboxes.push({
        title: checkboxCategory,
        checkboxList: CheckboxData,
        selected: [],
      });
    },
  },
  mounted() {
    this.filteredItems = datos.map((item) => {
      return item;
    });
    const value = this.search.charAt(0).toUpperCase() + this.search.slice(1);
    this.filteredItems.filter(function(item) {
      return (
        item.nombre.indexOf(value) > -1 ||
        item.id.indexOf(value) > -1 ||
        item.id.indexOf(value) > -1
      );
    });
    //making copy of items to be used in checkbox filter
    this.originalItems = this.filteredItems;

    //generate skin checkbox
    this.createCheckBox("hair");
    this.createCheckBox("skin");
    this.createCheckBox("hairColor");
    this.createCheckBox("eyes");
    this.createCheckBox("background");
    this.createCheckBox("accessory");
    this.createCheckBox("extra");
  },

  computed: {
    checkF() {
      return this.filteredItems.filter(function(item) {
        return item;
      });
    },
  },
};
</script>

<style lang="sass">

@import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@300;400;500;600;700&display=swap')

html, body
    font-family: 'Comfortaa', cursive !important

.cardID
    font-family: 'Comfortaa', cursive !important
    font-weight: 700 !important
    color: #555555
    font-size: 30px
    margin: 20px 0px 5px 0px
.cardName
    font-family: 'Comfortaa', cursive !important
    font-weight: 400
    color: #555555
    font-size: 30px
    margin:0px 0px 10px 0px

.v-main__wrap
    background-color: #DF57FF
.v-application
    line-height: 1 !important



.container_cards
    display: flex
    flex-wrap: wrap
    justify-content: center
.card_punky
    max-width: 300px
    margin: 10px
    cursor: pointer
    box-shadow:2px 2px 5px rgba(0,0,0,0.15)
    border-radius:15px !important
    transition: all 0.2s cubic-bezier(.56,.14,.28,.76)
.card_punky:hover
    box-shadow:5px 5px 15px rgba(0,0,0,0.45)
    transform: scale(1.05)


.card_inside
    padding: 10px

.buscador_div
    width:100%
    margin-bottom:-15px
    margin-top:15px
    padding:15px


.border_radius_a
    border-radius:15px !important

.textField
  font-family: 'Comfortaa', cursive !important
  font-weight: 800 !important
  color: #C9C9C9
  font-size: 35px

.v-input--selection-controls
    margin-top: 0px !important
    padding-top: 0px !important

.v-text-field.v-text-field--solo:not(.v-text-field--solo-flat) > .v-input__control > .v-input__slot
   box-shadow:         inset 5px 5px 10px rgba(0,0,0,0.20) !important


.text-center
  margin-left: auto
.btnRarity
  background-color: #fff !important
  border: solid 2px #fff !important
  font-family: 'Comfortaa', cursive !important
  font-weight: 500 !important
  border-radius: 15px !important
  color: #494949 !important
.btnDavinci
  margin-left: 10px
  background: linear-gradient(90deg, rgba(4,177,231,1) 0%, rgba(100,247,191,1) 100%) !important
  border: solid 2px #fff !important
  font-family: 'Comfortaa', cursive !important
  font-weight: 500 !important
  color: #fff

.v-btn
  text-transform: none !important
  letter-spacing: 0em !important

.navbar
  background-color: #5644C7 !important
  box-shadow: none !important


.logoPunky
  color: #fff !important
  font-family: 'Comfortaa', cursive !important
  font-weight: 800 !important
</style>
