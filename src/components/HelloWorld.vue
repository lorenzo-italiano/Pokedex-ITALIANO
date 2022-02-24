<template>
  <main>
    <section>
      <div v-if="displayInfos" id="infosView">
        <figure :style="{backgroundColor:colorPalette(this.pokemonToDisplay.color)}">
          <img :src="pokemonInfoSrc" alt="Big picture of concerned pokemon">
        </figure>
        <div id="infosRightSide">
          <div id="xcross">
            <img src="../assets/xcross.png" alt="information view exit cross" @click="this.displayInfos=false">
          </div>
          <div id="infosNavBar">
            <a @click.prevent="this.infoGeneral=true">Général</a>
            <a @click.prevent="this.infoGeneral=false">Stats</a>
          </div>
          <div id="infosGeneral" v-show="infoGeneral">
            <p>{{capitalizeFirstLetter(this.pokemonToDisplay.name)}}</p>
            <p>#{{ getThreeLetterId(this.pokemonToDisplay.id)}}</p>
            <div id="weightHeightInfo">
              <p>Weight:{{this.pokemonToDisplay.weight}}</p>
              <p>Height:{{this.pokemonToDisplay.height}}</p>
            </div>
            <div id="displayTypesDetails">
              <p v-for="type in this.pokemonToDisplay.types" :key="type.type.name" class="types">{{type.type.name}}</p>
            </div>
          </div>
          <div id="infosStats" v-show="!infoGeneral">
            <p v-for="stat in this.pokemonToDisplay.stats" :key="stat.stat.name">{{stat.stat.name}}: {{stat.base_stat}}</p>
          </div>
        </div>
      </div>

      <form @submit.prevent="fetchPokemonBySearch">
        <input type="text" class="searchBar" v-model="search" placeholder="Entrez le nom ou l'id d'un pokemon">
        <button type="submit">Search</button>
      </form>
      <div v-if="this.loadedImage!=this.imageMaxCount" class="loadingScreen">
        <img src="../assets/loading_gif.gif" alt="loading animation">
      </div>
      <div v-if="!loading">
        <ul id="pokemonListul" v-show="this.loadedImage>=this.imageMaxCount">
          <li v-for="pokemon in data.sort(GetSortOrder('id'))" v-bind:key="pokemon.id" :style="{backgroundColor:colorPalette(pokemon.color)}" @click="getPokemonInfos(pokemon)">
            <img :src="pokemon.src" :alt="pokemon.name" @load="this.loadedImage++">
            <h1>{{ capitalizeFirstLetter(pokemon.name) }}</h1>
            <h5>#{{ getThreeLetterId(pokemon.id) }}</h5>
            <div>
              <p v-for="type in pokemon.types" :key="type.type.name">{{type.type.name}}</p>
            </div>
          </li>
        </ul>
        <ol v-if="this.loadedImage==this.imageMaxCount" class="pageList">
          <li v-for="index in 22" :key="index" :style="{backgroundColor:isPageSelected(index)}" @click="fetchPokemonByPage(index);scrollToTop();">{{ index }}</li>
        </ol>
      </div>
    </section>
  </main>
  
</template>

<script>
import {ref, onMounted} from "vue"
import axios from 'axios'
export default {
  name: 'HelloWorld',
    methods: {
      GetSortOrder(prop) {    
        return function(a, b) {    
          if (a[prop] > b[prop]) {    
            return 1
          } else if (a[prop] < b[prop]) {    
            return -1 
        }    
        return 0   
      }
    },
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1)
    },
    getThreeLetterId(id){
      switch(id.toString().length){
        case 1:
          return "00"+id
        case 2:
          return "0"+id
        default:
          return id 
      }
    },      
    scrollToTop(){
      document.documentElement.scrollTop = 0
      console.log("scrolling to the top !")
    },
    fetchPokemonByPage(page){
      this.loading = true
      this.currentPage = page
      this.loadedImage = 0
      let img = 0
      axios
        .get("https://pokeapi.co/api/v2/pokemon/?limit=52&offset="+(page-1)*52)
        .then((response) => {
          while(this.data.length > 0){
            this.data.pop()
          }
          response.data.results.forEach(el => {
            img += 1
            let a = {}
            axios
              .get(el.url)
              .then((response) => {
                a = response.data
                //a.src = response.data.sprites.front_default
                a.src = this.getThreeLetterId(response.data.id) > 898 ? response.data.sprites.front_default : "https://assets.pokemon.com/assets/cms2/img/pokedex/detail/" + this.getThreeLetterId(response.data.id)  +".png" 
                axios
                  .get(response.data.species.url)
                  .then((resp) => {
                    a.color = resp.data.color.name
                    this.data.push(a)
                  })
              })
          })
          this.imageMaxCount = img
          // A la main parce qu'11 images ne se chargent pas sur cette page
          if(this.currentPage===21){
            this.imageMaxCount -= 11
          }else if(this.currentPage===20){
            this.imageMaxCount -= 2
          }
        })
        .then(() => {
          console.log(this.data)
          this.loading=false
        })

        .catch(err =>{
          console.log(err)
        })
      },
      fetchPokemonBySearch(){
        this.loading = true
        console.log(this.search)
        while(this.data.length > 0){
              this.data.pop()
        }
        if(this.search == ""){
          this.fetchPokemonByPage(this.page)
        }else{


        this.loadedImage = 0
        this.imageMaxCount = 1

        axios
          .get("https://pokeapi.co/api/v2/pokemon/"+this.search)
          .then((response) => {
            console.log(response.data)
            let a = {}
            a = response.data
            //a.src = response.data.sprites.front_default
            a.src = this.getThreeLetterId(response.data.id) > 898 ? response.data.sprites.front_default : "https://assets.pokemon.com/assets/cms2/img/pokedex/detail/" + this.getThreeLetterId(response.data.id)  +".png" 
            axios
              .get(response.data.species.url)
              .then((resp) => {
                a.color = resp.data.color.name
                this.data.push(a)
              })
            })
          .then(() => {
            console.log(this.data)
            this.loading=false
        })

        .catch(err =>{
          console.log(err)
        })
        }
      },
    getPokemonInfos(pokemon){
      this.pokemonToDisplay=pokemon
      console.log(pokemon.name)
      console.log(this.pokemonToDisplay)
      this.displayInfos = true
    },
    colorPalette : function(color){
        switch(color){
          case "blue":
            return "#adfafc"
          case "red":
            return "#fcadad"
          case "green":
            return "#defcad"
          case "yellow":
            return "#fafcad"
          case "brown":
            return "#c4a1a0"
          case "purple":
            return "#d8adfc"
          case "gray":
            return "#c4c4c4"
          case "pink":
            return "#fcadef"
          case "black":
            return "#4c4c4c"
          default:
            return color
        }
      },
      isPageSelected(index){
        if(index===this.currentPage){
          return "#ff6763"
        }
        else{
          return ""
        }
      }
      
  },
  setup(){
    const data = ref(null)
    const loading = ref(true)
    const error = ref(null)


    function getThreeLetterId(id){
      switch(id.toString().length){
        case 1:
          return "00"+id
        case 2:
          return "0"+id
        default:
          return id 
      }
    }

    function fetchData(){
      loading.value = true
      axios
        .get("https://pokeapi.co/api/v2/pokemon/?limit=52")
        .then(function(response) {
          console.log(response.data)
          data.value = []
          response.data.results.forEach(el => {
            let a = {}
            axios
              .get(el.url)
              .then(function(response){
                a = response.data
                //a.src = response.data.sprites.front_default
                a.src = getThreeLetterId(response.data.id) > 898 ? response.data.sprites.front_default : "https://assets.pokemon.com/assets/cms2/img/pokedex/detail/" + getThreeLetterId(response.data.id)  +".png" 
                axios
                  .get(response.data.species.url)
                  .then(function(resp){
                    a.color = resp.data.color.name
                    data.value.push(a)
                  })
              })
          })
          loading.value = false
        })
        .catch(err =>{
          console.log(err)
          loading.value = true
        })
      }

      onMounted(() =>{
        fetchData()
      })

      
      return{
        data,
        loading,
        error,
      }
    },
    data(){
      return{
        search: '',
        currentPage : 1,
        loadedImage : 0,
        displayInfos : false,
        pokemonToDisplay : {},
        infoGeneral : true,
        imageMaxCount : 52
      }
    },
    computed:{
      pokemonInfoSrc: function(){
        return this.pokemonToDisplay.id>=650 ? this.pokemonToDisplay.id>=894 ? this.pokemonToDisplay.src : "https://assets.pokemon.com/assets/cms2/img/pokedex/detail/" + this.getThreeLetterId(this.pokemonToDisplay.id)  +".png" : "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/"+ this.pokemonToDisplay.id +".svg"
      }
    }      
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

img{
  width:55%;
  height:auto;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}

.loadingScreen{
  display:flex;
  align-items:center;
  justify-content:center;
}

.pageList{
  display:flex;
  flex-direction:row;
  justify-content:center;
}

.pageList > li{
  display:flex;
  justify-content:center;
  align-items:center;
  border-radius:10rem;
  font-size:1em;
  width:2.5%;
  max-height: 5%;
  border:2px solid black;
  margin: 3vh 5px 0 5px;
}

.pageList > li:hover{
  cursor:pointer;
  background-color:black;
  color:white;
}

#infosView{
  position: fixed;
  display:flex;
  top:15%;
  left:5%;
  width:90%;
  height:80%;
  background-color:white;
  opacity:0.95;
  z-index:3;
}

#infosView > figure{
  display:flex;
  align-items:center;
  justify-content:center;
  height:100%;
}

#infosView > figure > img{
  max-width:100%;
  height:auto;
}

#infosRightSide{
  display:flex;
  flex-direction:column;
  grid-row-gap:0;
}

#xcross{
  height:5vh;
}

#xcross > img{
  height:100%;
  width:auto;
  float:right;
  padding:1vh;
}

#xcross > img:hover{
  cursor:pointer;
}

#infosNavBar{
  display:flex;
  height:10vh;
  align-items:center;
  justify-content:center;
  gap:5vw;
  font-size:2em;
}

#infosNavBar > a:hover{
  cursor:pointer;
  border-bottom:solid 3px black;
  transition:0.2s;
}

#infosNavBar > a{
  color:gray;
}

#infosGeneral{
  height:60vh;
  display:flex;
  flex-direction:column;
  grid-row-gap:0;
  font-size:2em;
  align-items:center;
  justify-content:center;
}

#infosGeneral > *{
  display:flex;
  justify-content:center;
  align-items:center;
  width:100%;
  max-height:10vh;
}

#infosStats{
  height:60vh;
  font-size: 2em;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
}

.types{
  border:2px solid black;
  border-radius:25%;
  padding: 2vh 2vw;
}

#displayTypesDetails{
  display:flex;
  gap: 0 2vw;
}

#weightHeightInfo{
  gap:5%;
}

/* Small devices (landscape phones, 576px and up)*/
@media (min-width: 200px) {
  #infosView{
    flex-direction:column;
  }
  #infosView > figure{
    height:30%;
    width:100%;
    border-bottom:3px solid black;
    border-right: 0;
  }
  #infosRightSide{
    height:70%;
    width:100%;
  }
  #infosView > figure > img{
    max-height:100%;
    width:auto;
  }
}
/* Small devices (landscape phones, 576px and up)*/
@media (min-width: 567px) {
  #infosView{
    flex-direction:column;
  }
  #infosView > figure{
    height:35%;
    width:100%;
    border-bottom:3px solid black;
    border-right: 0;
  }
  #infosRightSide{
    height:65%;
    width:100%;
  }
  #infosView > figure > img{
    max-height:100%;
    width:auto;
  }
}

/* Medium devices (tablets, 768px and up)*/
@media (min-width: 768px) {
  #infosView{
    flex-direction:row;
  }
  #infosView > figure{
    height:100%;
    width:50%;
    border-right:3px solid black;
    border-bottom: 0;
  }
  #infosRightSide{
    height:100%;
    width:50%;
  }
}

/* Extra large devices (large laptops and desktops, 1200px and up) */
@media only screen and (min-width: 1200px) {
  #infosView{
    flex-direction:row;
  }   
  #infosView > figure{
    height:100%;
    width:50%;
    border-right:3px solid black;
    border-bottom: 0;
  } 
  #infosRightSide{
    height:100%;
    width:50%;
  }
}

</style>
