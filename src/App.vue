<script>
import axios from 'axios';
import format from 'date-fns/format';

const apiKey = "5d4d61119308f54fa48796265f35f5a9";
export default {
  data() {
    return {
      movies: [],
      genres: [],
      dialog: false,
      format
    };
  },
  methods: {
    async getData() {
      try {
        const response = await axios.get(` https://api.themoviedb.org/3/movie/popular?api_key=${apiKey}`);

        console.log(response.data.results);
        this.movies = response.data.results;
      } catch (err) {
        console.log(err);
      }
    },
    // async getGenre(id) {
    //   try {
    //     const response = await axios.get(` https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}&language=en-US`);

    //     console.log(response.data.genres);
    //     this.genres = response.data.genres;
    //   } catch (err) {
    //     console.log(err);
    //   }
    // }
  },
  created() {
    this.getData();
    //this.getGenre();
  },
};
</script>
<template>
  <div class="container">
    <div class="card" v-for="movie in movies" :key="movie.id">
      <h2>{{ movie.original_title }}</h2>
      <img :src="'https://image.tmdb.org/t/p/w200' + movie.poster_path" :alt="movie.original_title" />
      <div class="text-center">
        <v-btn color="primary" @click="dialog = true">
          Read More
        </v-btn>

        <v-dialog v-model="dialog" width="auto">
          <v-card>
            <v-card-text>
              <p>{{ movie.overview }}</p>
              <span><b>Release Date:</b> {{ format(new Date(movie.release_date), "dd/MM/yyyy") }}</span>
            </v-card-text>
            <v-card-actions>
              <v-btn color="primary" block @click="dialog = false">Close</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </div>
  </div>
</template>