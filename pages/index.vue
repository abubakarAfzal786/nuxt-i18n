<template>
  <v-container fluid>
    <v-row align="center">
      <v-col class="d-flex" cols="12">
        <p>Current language: {{ $i18n.locale }}</p>
        <nuxt-link
          v-for="locale in availableLocales"
          :key="locale.code"
          :to="switchLocalePath(locale.code)"
          >{{ locale.name }}</nuxt-link
        >

        <h2 class="subtitle">{{ $t("welcome") }}</h2>
      </v-col>
      <v-col class="d-flex" cols="12">
        <v-card class="overflow-hidden">
          <!-- <v-row align="center"> -->
          <v-toolbar color="teal" dark>
            <v-app-bar-nav-icon></v-app-bar-nav-icon>

            <v-toolbar-title>{{ $t("topics") }}</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>

          <v-list>
            <v-list-group
              v-for="(item, index) in menuItems"
              :key="index"
              :prepend-icon="item.action"
              no-action
            >
              <template v-slot:activator>
                <v-list-item-content>
                  <v-list-item-title
                    v-text="item['name_' + $i18n.locale]"
                  ></v-list-item-title>
                </v-list-item-content>
              </template>

              <v-list-item
                v-for="(child, index) in item.sub_menus"
                :key="index"
              >
                <v-list-item-content>
                  <v-list-item-title
                    v-text="child['name_' + $i18n.locale]"
                  ></v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-group>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      selectedValue: "",
      items: ["Foo", "Bar", "Fizz", "Buzz"],
      menuItems: [],
    };
  },
  computed: {
    availableLocales() {
      return this.$i18n.locales.filter((i) => i.code !== this.$i18n.locale);
    },
  },
  created() {
    this.selectedValue = this.$i18n.locale;
    axios
      .get("https://admin.bookcab.fr/menus")
      .then((response) => {
        console.log(response.data);
        this.menuItems = response.data;
        this.menuItems[0];
      })
      .catch((error) => {
        console.log(error.message);
      });
  },
  methods: {
    onChange(event) {
      this.$router.replace(this.switchLocalePath(event));
    },
  },
};
</script>

<style scoped>
.subtitle {
  font-weight: 300;
  font-size: 3em;
  color: #2e495e;
  word-spacing: 5px;
  padding-bottom: 15px;
  margin-top: 200px;
}
</style>
