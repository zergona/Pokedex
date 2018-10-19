<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <div id="Pokemon">
          <form v-on:submit.prevent="pokeSubmit(ime)">
            <input type="text" v-model="ime" placeholder="Pokemon name">
            <v-btn type="submit"> 
              Search
            </v-btn>            
                  
          </form>
          <body style="padding-top: 25px;">
            <img v-bind:src = "spriteurl">
            <p>Pokemon name: {{ pokeName }}</p>
            <p>Pokemon ID: {{ pokeID }}</p>
            <p>Pokemon type(s): {{ pokeType1 }}</p>
            <p>{{ pokeType2 }}</p>
            <p>Moves: </p>
            <div v-for="move in moves">
            <b-table striped hover>{{ move }}</b-table>
            </div>
          </body>
        </div>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
  import { data } from "../App.vue"
  import {pokeSubmit} from "../App.vue"
  import vue from 'Vue'
  import $ from 'jquery'
  import App from "../App.vue"

  export default {
    name: 'Pokemon',
    data: function () {
      return {
        podaci: '',
        ime: '',
        pokeID: '',
        pokeName: '',
        pokeType1: '',
        pokeType2: '',
        spriteurl: '',
        moves: [],
        
    }
  },
    methods: {
      pokeSubmit (ime) {
        var pokeURL = "http://pokeapi.salestock.net/api/v2/pokemon/" + ime;
        $.getJSON(pokeURL).then(podaci =>{
          this.pokeID = podaci.id,
          this.pokeName = podaci.name,
          this.pokeType1= podaci.types[0].type.name,
          this.moves = []
          for(var i = 0; i<podaci.moves.length; i++){
            this.moves[i] = podaci.moves[i].move.name
          }
          if (podaci.types.length == 2) {
            this.pokeType2 = podaci.types[1].type.name;
          }
          else {this.pokeType2 = null;}
          this.spriteurl = podaci.sprites.front_default

          console.log("Number: ", this.pokeID)
          console.log("Name: ", this.pokeName)
          console.log("Type 1: ", this.pokeType1)
          console.log("Type 2: ", this.pokeType2)
          for(var i = 0; i<podaci.moves.length; i++){
            console.log("Move:", this.moves[i])
          }
          console.log("Sprite: ", this.spriteurl);
        })
      },
      log: function (message) {
      this.$log(`${new Date().toLocaleTimeString()} -- ${message}`)
    }

  }
  }
  
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
