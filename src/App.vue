<template>
  <v-app>
    <v-row align="center" justify="center">
      <v-col cols="12" sm="6">
        <v-text-field
          label="Search for pictures"
          v-model.lazy.trim="searchString"
          @change="loadPics"
          placeholder="Search for pictures"
          solo
          class="mt-9"
        ></v-text-field>
        <v-slider
          v-model="picSize"
          label="Size"
          min="2"
          max="12"
        ></v-slider>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12" sm="10" offset-sm="1">
        <v-card>
          <v-container fluid>
            <v-row>
              <v-col
                v-for="(picture,idx) in pictures"
                :key="idx"
                class="d-flex child-flex"
                :cols="picSize"
              >
                <v-card elevation="4" tile class="d-flex">
                  <v-img
                    :src="picture.webformatURL"
                    :lazy-src="picture.previewURL"
                    aspect-ratio="1"
                    class="grey lighten-5"
                  >
                    <template v-slot:placeholder>
                      <v-row class="fill-height ma-0" align="center" justify="center">
                        <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                      </v-row>
                    </template>
                  </v-img>
                </v-card>
              </v-col>
            </v-row>
          </v-container>
        </v-card>
      </v-col>
    </v-row>

  </v-app>
</template>

<script>
export default {
  name: "App",

  data: function () {
    return {
      searchString: "",
      picSize: 3,
      pictures: [],
    };
  },

  mounted: function () {
    this.loadPics(this.searchString);
  },

  methods: {
    loadPics: function () {
      const API_KEY = "17899782-74bede5cece1cf896ff273166";
      const URL =
        "https://pixabay.com/api/?key=" +
        API_KEY +
        "&q=" +
        encodeURIComponent(this.searchString) +
        "&per_page=40&image_type=photo";
      fetch(URL)
        .then((response) => response.json())
        .then((response) => {
          this.pictures = Array.from(response.hits);
          console.log(this.pictures);
        });
    },
  },
};
</script>
