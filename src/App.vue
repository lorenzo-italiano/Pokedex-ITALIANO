<template>
    <div >
        <header>
            <h1>PokéList</h1>
            <nav>
                <a>Home</a>
                <a>Link1</a>
                <a>Link2</a>
                <a>Link3</a>
            </nav>
        </header>    
    
        <HelloWorld />
    
        <footer>
            <div>
                <h3>Data from pokeapi</h3>
                <a>https://pokeapi.co/</a>
            </div>
            <img id="logopolytech" src="./assets/logopolytech.png" alt="logo de Polytech">
            <p id="copyright">&copy; ITALIANO Lorenzo</p>
        </footer>
    </div>  
</template>
<script>
import HelloWorld from './components/HelloWorld.vue'



export default {
  name: 'App',
  components: {
    HelloWorld
  },
  methods:{
  }
}
</script>

<style>
        @import url('https://fonts.googleapis.com/css2?family=Anton&family=Oswald:wght@200;400;700&display=swap');  
        /* RESET */
html{
    scroll-behavior: smooth;
}

* {  
	vertical-align: baseline;  
	font-family: "Oswald";  
	font-style: inherit;  
	font-size: 100%;  
	padding: 0;  
	margin: 0;  
	}   
table {  
	border-collapse: collapse;  
	border-spacing: 0;  
}

ul{
    list-style-type: none;
}
/* Begin */
body{
    display: grid;
    max-width: 100vw;
    min-height: 100vh;
    /*background-image: linear-gradient(to right, #212120, #20201f, #1e1e1d, #1d1d1c, #1c1c1b, #1c1c1b, #1c1c1b, #1c1c1b, #1d1d1c, #1e1e1d, #20201f, #212120);
  */
    background-color:#f2f2f2;
}

header{
    position:sticky;
    top: 0;
    height: 10vh;
    background-image: linear-gradient(to right, #d1e153, #dcd341, #e6c332, #efb428, #f6a325, #ff9150, #ff8674, #ff8394, #f397cb, #d6b2f0, #b9caff, #b0dcfc);
    display: flex;
    flex-direction: row;
    align-items: center;
    padding: 0 10vw;
    z-index:3;
}

header > h1{
    font-size: 2.5em;
}

header > nav{
    display: flex;
    gap: 1em ;
    position: absolute;
    right: 10vw;
    font-size: 2em;
}

header > nav > *:hover{
    cursor:pointer;
    opacity: 0.5;
}

main{
    min-height: 70vh;
    margin: 5vh 15vw;
}

footer{
    display:flex;
    align-items:center;
    justify-content:center;
    gap:5vh 5vw;
    height: 20vh;
    background-image: linear-gradient(to right, #d1e153, #dcd341, #e6c332, #efb428, #f6a325, #ff9150, #ff8674, #ff8394, #f397cb, #d6b2f0, #b9caff, #b0dcfc);
}

h1{
    font-family: "Anton";
}

button {
    border-radius: 100rem;
    padding: 1rem;
    font-size: 1rem;
    padding: .5rem 3rem;
    color: black;
    box-shadow: 0 0 6px 0 rgba(157, 96, 212, 0.5);
    border: solid 3px transparent;
    background-image: linear-gradient(rgba(255, 255, 255, 0), rgba(255, 255, 255, 0)), linear-gradient(to right, #d1e153, #dcd341, #e6c332, #efb428, #f6a325, #ff904c, #ff816e, #ff7b8c, #f48ac3, #d2a2ed, #a6baff, #85ccff);
    
    /*background-image: #f2f2f2,linear-gradient(to right, #d1e153, #dcd341, #e6c332, #efb428, #f6a325, #ff904c, #ff816e, #ff7b8c, #f48ac3, #d2a2ed, #a6baff, #85ccff);
*/
    background-origin: border-box;
    background-clip: content-box, border-box;
    box-shadow: 2px 1000px 1px #f2f2f2 inset;
  }
  
button:hover {
    box-shadow: none;
    cursor:pointer;
}

input.searchBar{
    border-radius: 100rem;
    padding: 1rem;
    font-size: 1rem;
    padding: .5rem 3rem;
    box-shadow: 0 0 6px 0 rgba(157, 96, 212, 0.5);
    border: solid 3px transparent;
    background-image: linear-gradient(rgba(255, 255, 255, 0), rgba(255, 255, 255, 0)), linear-gradient(to right, #d1e153, #dcd341, #e6c332, #efb428, #f6a325, #ff904c, #ff816e, #ff7b8c, #f48ac3, #d2a2ed, #a6baff, #85ccff);
    background-origin: border-box;
    background-clip: content-box, border-box;
    box-shadow: 2px 1000px 1px #f2f2f2 inset;
}

section{
    display: flex;
    flex-direction: column;
    gap: 10vh 0;
}

section > form{
    display: flex;
    justify-content: center;
    gap:1vw;
}
section > form > input{
    width: 70%;
}

#pokemonListul{
    width: 100%;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
}

#pokemonListul > li{
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    height: 35vh;
    border-radius: 10%;
    margin: 1vh 1vw;
    border:solid black 2px;
}

#pokemonListul > li > div{
    display:flex;
    gap:1vw;
}

#pokemonListul > li > div > p {
    border:2px solid black;
    padding:0.5vh 0.5vw;
    border-radius: 25%;
}

#pokemonListul > li h1{
  font-family:"Oswald";
  font-weight:400;
}

#pokemonListul > li:hover{
    cursor:pointer;
    border: 0;
    transform:scale(1.03);
    transition: 0.3s;
    transition-timing-function: ease-in-out;
    position: relative;
}

/* Small devices (landscape phones, 576px and up)*/
@media (min-width: 200px) {
    #logopolytech{
        display:none;
    }
    main{
        margin: 10px 10%;
    }
    header > nav{
        display:none;
    }
    #pokemonListul{
        grid-template-columns: 1fr;
    }
    #pokemonListul > li > img{
        width:30%;
        height:auto;
    }
    body{
        overflow-x:hidden;
    }
}
/* Small devices (landscape phones, 576px and up)*/
@media (min-width: 567px) {
    #logopolytech{
        display:none;
    }
    main{
        margin: 2vh 30px;
    }
    header > nav{
        display:none;
    }
    #pokemonListul{
        grid-template-columns: 1fr 1fr;
    }
    #pokemonListul > li > img{
        width:50%;
        height:auto;
    }
 }

/* Medium devices (tablets, 768px and up)*/
@media (min-width: 768px) {
    #logopolytech{
        display:block;
    }
    main{
        margin: 2vh 10vw;
    }
    header > nav{
        display:flex;
    }
    #pokemonListul{
        grid-template-columns: 1fr 1fr 1fr;
    }
    #pokemonListul > li > img{
        width:50%;
        height:auto;
    }
}

/* Extra large devices (large laptops and desktops, 1200px and up) */
@media only screen and (min-width: 1200px) {
    #logopolytech{
        display:block;
    }
    main{
        margin: 5vh 15vw;
    }
    header > nav{
        display:flex;
    }
    #pokemonListul{
        grid-template-columns: 1fr 1fr 1fr 1fr;
    }
    #pokemonListul > li > img{
        width:50%;
        height:auto;
    }
 }
 
 
</style> 