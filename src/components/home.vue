<template>
  <div class="main-content">
    <div class="title">
      <h2>Ghibli Movie Library</h2>
    </div>
    <div class="container">
      <table id="table--films">
        <thead>
          <tr>
            <th class="text-center">Title</th>
            <th class="text-center">Director</th>
            <th class="text-center">Producer</th>
            <th class="text-center">Release Date</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="film in films">
            <td><a href="#" @click="getFilmByID(film.id)">{{film.title}}</a></td>
            <td>{{film.director}}</td>
            <td>{{film.producer}}</td>
            <td>{{film.release_date}}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="modal" tabindex="-1" role="dialog" v-if="show == true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h3 class="modal-title"><b>{{ title }}</b></h3>
          </div>
          <div class="modal-body">
            <div class="film_detail">
              <b>Description</b>
              <p>{{ description }}</p>
            </div>
            <div class="film_detail">
              <b>Director</b>
              <p>{{ director }}</p>
            </div>
            <div class="film_detail">
              <b>Producer</b>
              <p>{{ producer }}</p>
            </div>
            <div class="film_detail">
              <b>Release Date</b>
              <p>{{ release_date }}</p>
            </div>
            <div class="film_detail">
              <b>Rating Score</b>
              <p>{{ rt_score }}</p>
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="closeModal()">Close</button>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import main from "../main.js";  
import axios from 'axios';

export default {
  data () {
    return {
      films: [],
      show: false,
      title: "",
      description: "",
      director: "",
      producer: "",
      release_date: "",
      rt_score: "",
    }
  },
  mounted () {
    this.getListFilms()
  },
  methods: {
    getListFilms: function(){
      axios.get('https://ghibliapi.herokuapp.com/films').then((response)=>{
          if(response.status == 200){
              this.films = response.data
          }
      }).catch(function (error) {
          return error
      });
    },

    getFilmByID: function(id){             
      axios.get('https://ghibliapi.herokuapp.com/films/'+id).then((response)=>{
          if(response.status == 200){
              this.show = true
              this.title = response.data.title
              this.description = response.data.description
              this.director = response.data.director
              this.producer = response.data.producer
              this.release_date = response.data.release_date
              this.rt_score = response.data.rt_score
          }
      }).catch(function (error) {
          return error
      });
    },

    closeModal: function(){
      this.show = false              
    }
  }
};
</script>