<template>
  <v-app id="inspire">
    <v-navigation-drawer
      app
      permanent
    >
    <v-card>
      <v-card-title>
        Filters
      </v-card-title>
    </v-card>
    <v-expansion-panels>
      <v-expansion-panel
        v-for="(item,i) in 10"
        :key="i"
      >
          <v-expansion-panel-header>
            Skin
          </v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-checkbox
              v-model="selected"
              label="Honey"
              value="Honey"
            ></v-checkbox>
            <v-checkbox
              v-model="selected"
              label="Almond"
              value="Almond"
            ></v-checkbox>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-navigation-drawer>

    <v-app-bar app>
      

      <v-toolbar-title>PUNKY GANG</v-toolbar-title>

      <v-text-field
        label="Prepend inner"
        prepend-inner-icon="mdi-map-marker"
      ></v-text-field>


      <div class="text-center">
        <v-menu offset-y>
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              v-bind="attrs"
              v-on="on"
            >
              Dropdown
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

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>



<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    items: [
        { title: 'Click Me' },
        { title: 'Click Me' },
        { title: 'Click Me' },
        { title: 'Click Me 2' },
      ],
  }),
};
</script>
