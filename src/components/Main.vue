<template>
  <v-container class="container_cards" v-bind:style="{ backgroundColor: this.mycolor}" id="app">
    <!-- NAVIGATION DRAWER -->
    <v-navigation-drawer v-model="drawer" app class="navigation_drawer" width="325">

        <div class="filter_box">
          <h3 class="filters_title">
            <v-icon left dark>mdi-filter-variant</v-icon>
            FILTER
          </h3>
          
        </div>

      <v-expansion-panels accordion flat>
        <v-expansion-panel class="expansion_box" v-for="(cb, cbindex) in checkboxes" :key="cb.title">
          <v-expansion-panel-header class="expansion_title">{{ cb.titleLB }}</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-checkbox
              v-model="cb.selected"
              v-for="(chkbox, index) in cb.checkboxList"
              :label="chkbox.label + ` (${chkbox.number})`"
              :value="chkbox.value"
              @change="checkboxFilter(cbindex)"
              :key="index"
              class="checkboxes"
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

          <v-list rounded class="list_" >
            <v-list-item link class="list_item"
              ><v-list-item-title v-on:click="cambiarOrg('newest')"
                >Newest</v-list-item-title
              ></v-list-item
            >
            <v-list-item link class="list_item"
              ><v-list-item-title v-on:click="cambiarOrg('oldest')"
                >Oldest</v-list-item-title
              ></v-list-item
            >
            <v-list-item link class="list_item"
              ><v-list-item-title v-on:click="cambiarOrg('mostrare')"
                >Most Rare</v-list-item-title
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
      <img src="../assets/davinci.svg" width="20px" alt="davinci" class="davinci_svg">
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
        @click:clear="callItBack()"
      ></v-text-field>
    </div>


    <!-- v-bind:href="item.url" -->

    <div v-for="item in filteredItems" v-bind:key="item.id" class="card_punky">
      <v-card
        class="card_inside border_radius_a"
        max-width="300"
        v-on:click="openDialog(item)"
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

        <!--
        <p class="text-center cardRarity">
         Rarity Rank: {{ item.rarityRank }}
        </p>
        -->
      </v-card>
    </div>


    <v-dialog
      v-model="dialog"
      max-width="900"
    >

      <v-card>
        <div class="dialog_container">
          <div class="dialog_izq">
            <v-img
              class="dialog_img"
              :src="dialogImgBig"
              :lazy-src="dialogImg"
            ></v-img>
            <h1 class="text-center cardID">#{{ this.dialogID }}</h1>
            <h2 class="text-center cardName">{{ this.dialogNombre }}</h2>
            <v-btn
              dark
              rounded
              target="_blank"
              :href="dialogURL"
              class="btnDavinciDialog"
              block
            >
            <img src="../assets/davinci.svg" width="20px" alt="davinci" class="davinci_svg">
               View on Davinci
            </v-btn>
          </div>
          <div class="dialog_der">
            <div class="dialog_attributes">
              <h3>ATTRIBUTES</h3>
            </div>
               <v-simple-table  class="tabla">
                <template >
                  <tbody>
                    <tr><td>SKIN</td><td class="bl">{{ this.dialogSkin }}</td></tr>
                    <tr><td>HAIR</td><td class="bl">{{ this.dialogHair }}</td></tr>
                    <tr><td>HAIR COLOR</td><td class="bl">{{ this.dialogHairColor }}</td></tr>
                    <tr><td>EYES</td><td class="bl">{{ this.dialogEyes }}</td></tr>
                    <tr><td>RIGHT EAR</td><td class="bl">{{ this.dialogRightEar }}</td></tr>
                    <tr><td>LEFT EAR</td><td class="bl">{{ this.dialogLeftEar }}</td></tr>
                    <tr><td>MOUTH</td><td class="bl">{{ this.dialogMouth }}</td></tr>
                    <tr><td>BACKGROUND</td><td class="bl">{{ this.dialogBackground }}</td></tr>
                    <tr><td>ACCESSORY</td><td class="bl">{{ this.dialogAccessory }}</td></tr>
                    <tr><td>EXTRA</td><td class="bl">{{ this.dialogExtra }}</td></tr>
                  </tbody>
                </template>
              </v-simple-table>

              <div class="dialog_rank_cont">
                <div>
                  <h3 class="dialog_rank_rarity">RARITY RANK</h3>
                  <h3 class="dialog_rank_number">{{this.dialogRarityRank}}</h3>
                </div>
              </div>

          </div>
        
          <v-btn
            class="dialog_close"
            fab
            dark
            small
            color="red"
            @click="dialog = false"
          >
            <v-icon dark>
              mdi-close
            </v-icon>
          </v-btn>

        </div>

        

      </v-card>


    </v-dialog>
    


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
    org: "Oldest",
    orgID: "item.DocID",
    drawer: null,
    search: "",
    attributes: {
      Skin: "Skin",
    },

    mycolor:Math.round((Math.random()* (+2 + +10) + +1)),

    dialog: false,
    dialogID: "",
    dialogNombre: "",
    dialogURL: "",
    dialogImg: "",
    dialogImgBig: "",

    filteredItems: [],
    originalItems: [],
    checkboxes: [],
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
    callItBack(){
      this.search = ""
    },
    openDialog(item){
      console.log(item)
      this.dialog = true
      this.dialogID = item.id
      this.dialogNombre = item.nombre
      this.dialogURL = item.url
      this.dialogImg = item.img
      this.dialogImgBig = item.imgBig
      this.dialogRarityRank = item.rarityRank

      this.dialogSkin = item.skin
      this.dialogHair = item.hair
      this.dialogHairColor = item.hairColor
      this.dialogEyes = item.eyes
      this.dialogRightEar = item.rightEar
      this.dialogLeftEar = item.leftEar
      this.dialogMouth = item.mouth
      this.dialogBackground = item.background
      this.dialogAccessory = item.accessory
      this.dialogExtra = item.extra
    },

    cambiarOrg(value) {
      if (value == "newest") {
        this.org = "Newest";
        this.filteredItems = this.filteredItems.sort(function(a, b) {
          return b.id - a.id;
        });
      }
      if (value == "oldest") {
        this.org = "Oldest";
        this.filteredItems = this.filteredItems.sort(function(a, b) {
          return a.id - b.id;
        });
      }
      if (value == "mostrare") {
        this.org = "Most Rare";
        this.filteredItems = this.filteredItems.sort(function(a, b) {
          return a.rarity - b.rarity;
        });
      }
      console.log(this.filteredItems)
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
              item.rightEar.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "rightEar") ||
            (this.checkboxes[index].selected.includes(
              item.leftEar.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "leftEar") ||
            (this.checkboxes[index].selected.includes(
              item.mouth.toLowerCase().trim()
            ) &&
              this.checkboxes[index].title == "mouth") ||
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
        titleLB: checkboxCategory.replace(/([A-Z])/g, ' $1')
         // uppercase the first character
         .replace(/^./, function(str){ return str.toUpperCase(); }),
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
    this.createCheckBox("skin");
    this.createCheckBox("hair");
    this.createCheckBox("hairColor");
    this.createCheckBox("eyes");
    this.createCheckBox("rightEar");
    this.createCheckBox("leftEar");
    this.createCheckBox("mouth");
    this.createCheckBox("background");
    this.createCheckBox("accessory");
    this.createCheckBox("extra");


    if(this.mycolor == 2){
      this.mycolor = "#9DFF56"
    }if(this.mycolor == 3){
      this.mycolor = "#57FFB4"
    }if(this.mycolor == 4){
      this.mycolor = "#57B1FF"
    }if(this.mycolor == 5){
      this.mycolor = "#DF57FF"
    }if(this.mycolor == 6){
      this.mycolor = "#FF5696"
    }if(this.mycolor == 7){
      this.mycolor = "#6E57FF"
    }if(this.mycolor == 8){
      this.mycolor = "#1C1C1C"
    }if(this.mycolor == 9){
      this.mycolor = "#ECECEC"
    }if(this.mycolor == 10){
      this.mycolor = "#FF5656"
    }if(this.mycolor == 11){
      this.mycolor = "#FF8D57"
    }if(this.mycolor == 12){
      this.mycolor = "#FFCE57"
    }

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


//.v-main__wrap
//    background: #DF57FF

.v-application
    line-height: 1 !important



.container_cards
    display: flex
    flex-wrap: wrap
    justify-content: center
    height: 100%
.card_punky
    max-width: 300px
    max-height: 365px
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
  color: #C9C9C9 !important
  font-size: 20px !important

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

.navigation_drawer
  background-color: #6E57FF !important

.v-navigation-drawer__content
  padding: 15px

.expansion_box
  margin-bottom:5px
  box-shadow: 0px 0px 0px rgba(0,0,0,0) !important
  border-radius: 15px !important

.expansion_title
  color: #525252 !important
  font-family: 'Comfortaa', cursive !important
  font-weight: 600 !important

.filter_box
  padding:10px
  margin-bottom: 10px
.filters_title
  color: #fff !important
  font-family: 'Comfortaa', cursive !important
  font-weight: 800 !important
  background-color: none !important

.checkboxes
  color: #fff !important
  font-family: 'Comfortaa', cursive !important
  font-weight: 400 !important
  font-size: 10px !important
  height: 30px

.cardRarity
  font-family: 'Comfortaa', cursive !important
  font-weight: 200 !important
  font-size: 10px !important
  position: absolute
  bottom: -5px


/* width */
::-webkit-scrollbar 
  width: 5px

/* Track */
::-webkit-scrollbar-track 
  background: #6E57FF
  border-radius: 5px


/* Handle */
::-webkit-scrollbar-thumb 
  background: #fff


/* Handle on hover */
::-webkit-scrollbar-thumb:hover 
  background: #fff

@media (min-width: 0px)
.container 
    max-width: 1900px !important


.v-sheet.v-card
  border-radius: 20px !important



.v-dialog 
  border-radius: 20px !important

.dialog_container
  padding: 20px
  display: flex
  flex-direction: row

.dialog_izq
  width: 60%
.dialog_der
  width: 40%
  padding-left:10px
  display: flex
  flex-direction: column
.dialog_img
  border-radius: 15px !important

@media (max-width: 425px) 
  .dialog_container
    flex-direction: column
  .dialog_izq
    width: 100%
  .dialog_der
    width: 100%
    padding-left:0px
  .btnDavinciDialog
    margin-bottom: 20px !important 

.btnDavinciDialog
  background: linear-gradient(90deg, rgba(4,177,231,1) 0%, rgba(100,247,191,1) 100%) !important
  border: solid 2px #fff !important
  font-family: 'Comfortaa', cursive !important
  font-weight: 500 !important
  margin-bottom: 0px

.tabla
  font-family: 'Comfortaa', cursive !important
  color: #555 !important
  font-weight: 600
  border: thin solid rgba(0, 0, 0, 0.12)
  border-radius:15px !important
.bl
  border-left: thin solid rgba(0, 0, 0, 0.12)
  font-weight: 200 !important

.davinci_svg
  margin-right:5px

.dialog_attributes
  background: #828282
  padding: 10px 0px 10px 0px
  font-family: 'Comfortaa', cursive !important
  color: #fff !important
  font-weight: 600
  margin-bottom: 10px
  border-radius: 10px
  text-align: center
  font-size: 15px

.dialog_rank_cont
  font-family: 'Comfortaa', cursive !important
  color: #7B7B7B !important
  font-weight: 200
  text-align: center
  display: flex
  justify-content: center
  align-items: center
  height: 100%
.dialog_rank_rarity
  font-size: 15px
  margin-top: 15px
.dialog_rank_number
  font-size: 50px
  margin-top: 5px
  font-weight: 200 !important


.dialog_close
  position: absolute !important
  right: 10px
  top: 10px


.v-menu__content
  box-shadow: none !important 
.theme--light.v-list
  background: rgba(0,0,0,0) !important
.list_
  font-family: 'Comfortaa', cursive !important
  color: #7B7B7B !important
  font-weight: 200
.list_item
  background: #fff
  box-shadow: 0px 0px 10px rgba(0,0,0,0.3)
  text-align: center
  border: solid 2px white


.v-input textarea 
  color: rgb(70 70 70 / 87%) !important
  font-size: 20px !important

</style>
