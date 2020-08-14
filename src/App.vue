<template>
  <v-app>

    <v-text-field
        label="Search pictures"
        v-model="searchString"
        placeholder="..." class="ma-12"
    ></v-text-field>
    <div v-for="(picture,idx) in pictures" :key="idx">
      <p>{{picture.previewURL}}</p>
      <v-img    :src="picture.previewURL"></v-img>
    </div>


  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: function () {
    return {
      searchString: '',
      pictures: []
    }
  },

  mounted: function () {
    this.loadPics();
  }
  ,

  methods: {
    loadPics: function (line = '') {
      const API_KEY = '17899782-74bede5cece1cf896ff273166';
      const URL = "https://pixabay.com/api/?key=" + API_KEY + "&q=" + encodeURIComponent(line);

      fetch(URL)
          .then(response => response.json())
          .then(response => {
            this.pictures = Array.from(response.hits);
            console.log(this.pictures);
          })
    }
  }
}

</script>
