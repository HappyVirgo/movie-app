<template>

  <v-container v-if='loading'>
    <div class='text-xs-center'>
      <v-progress-circular
        indeterminate
        :size='150'
        :width='8'
        color='green'>
      </v-progress-circular>
    </div>
  </v-container>

  <v-container v-else grid-list-xl>
    <v-layout wrap>
      <v-flex xs4
        v-for='(item, index) in wholeResponse'
        :key='index'
        mb-2>
        <v-card>
          <v-img
            :src='item.Poster'
            aspect-ratio='1'
          ></v-img>

          <v-card-title primary-title>
            <div>
              <h2>{{item.Title}}</h2>
              <div>Year: {{item.Year}}</div>
              <div>Type: {{item.Type}}</div>
              <div>IMDB-id: {{item.imdbID}}</div>
            </div>
          </v-card-title>

          <v-card-actions class='justify-center'>
            <v-btn flat
              color='green'
              @click='singleMovie(item.imdbID)'
              >View</v-btn>
          </v-card-actions>

        </v-card>
      </v-flex>
  </v-layout>
  </v-container>
</template>

<script>
import movieApi from '@/services/MovieApi'

export default {
  data () {
    return {
      wholeResponse: [],
      loading: true
    }
  },
  mounted () {
    // this.wholeResponse = [
    //   {
    //     'Title': 'Indiana Jones and the Last Crusade',
    //     'Year': '1989',
    //     'imdbID': 'tt0097576',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BMjNkMzc2N2QtNjVlNS00ZTk5LTg0MTgtODY2MDAwNTMwZjBjXkEyXkFqcGdeQXVyNDk3NzU2MTQ@._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'Indiana Jones and the Temple of Doom',
    //     'Year': '1984',
    //     'imdbID': 'tt0087469',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BMGI1NTk2ZWMtMmI0YS00Yzg0LTljMzgtZTg4YjkyY2E5Zjc0XkEyXkFqcGdeQXVyNzkwMjQ5NzM@._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'Indiana Jones and the Kingdom of the Crystal Skull',
    //     'Year': '2008',
    //     'imdbID': 'tt0367882',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BZDIzNzM5MDUtZmI5MC00NGQ5LWFlNzEtYzE3ODIxNDI3ZmNhXkEyXkFqcGdeQXVyNjQ4ODE4MzQ@._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'Indiana Jones and the Temple of the Forbidden Eye Ride',
    //     'Year': '1995',
    //     'imdbID': 'tt0764648',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BMzk5ZmEyMTgtYzQ0ZC00YTEwLWExOTUtMDZhZmY5NDQ0OGJlXkEyXkFqcGdeQXVyNzAyNzI4Njc@._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'The Adventures of Young Indiana Jones: Treasure of the Peacocks Eye',
    //     'Year': '1995',
    //     'imdbID': 'tt0115031',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BOTg2MTc2NDAzOF5BMl5BanBnXkFtZTcwODExNDIyMQ@@._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'The Adventures of Young Indiana Jones: Travels with Father',
    //     'Year': '1996',
    //     'imdbID': 'tt0154003',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BMjU0NTY3M2UtZGJlYi00MDk4LWE0ZTAtZWI1MzcyZDg3Y2U5XkEyXkFqcGdeQXVyMzU0NzkwMDg@._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'Mr. Plinketts Indiana Jones and the Kingdom of the Crystal Skull Review',
    //     'Year': '2011',
    //     'imdbID': 'tt6330122',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BNGM4NDA3YTMtMWRiMy00MDgxLTkzMWItMDVjN2EzZmUwYjkxL2ltYWdlL2ltYWdlXkEyXkFqcGdeQXVyMTM3NzQ5NzQ@._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'The Adventures of Young Indiana Jones: Attack of the Hawkmen',
    //     'Year': '1995',
    //     'imdbID': 'tt0154004',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BMTcwMTU5NjExMV5BMl5BanBnXkFtZTYwNzU3MTA5._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'Monrovia, Indiana',
    //     'Year': '2018',
    //     'imdbID': 'tt8749146',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BMTU0MTQ0NTI2Ml5BMl5BanBnXkFtZTgwNjAzMjE1NjM@._V1_SX300.jpg'
    //   },
    //   {
    //     'Title': 'The Adventures of Young Indiana Jones: Daredevils of the Desert',
    //     'Year': '1999',
    //     'imdbID': 'tt0275186',
    //     'Type': 'movie',
    //     'Poster': 'https://m.media-amazon.com/images/M/MV5BZDQ0NGI4ZjktYmQyNC00ZDZhLTgyMGYtYjU1Zjg5N2YzZDRlXkEyXkFqcGdeQXVyNjExODE1MDc@._V1_SX300.jpg'
    //   }
    // ]
    this.loading = false
    this.wholeResponse = movieApi.fetchMovieCollection('indiana')
    this.loading = false
  },
  methods: {
    singleMovie (id) {
      this.$router.push('/movie/' + id)
    }
  }
}
</script>

<style lang='stylus' scoped>
  .v-progress-circular
    margin: 1rem
</style>
