<script>
import axios from 'axios';

export default {
  data() {
    return {
      characters: [],
    };
  },
  mounted() {
    this.fetchCharacters('https://rickandmortyapi.com/api/character/');
  },
  methods: {
    fetchCharacters(url) {
      axios.get(url)
        .then(response => {
          // Concatenar los personajes actuales con los nuevos
          this.characters = this.characters.concat(response.data.results);

          // Verificar si hay más páginas y hacer la solicitud recursiva si es necesario
          if (response.data.info.next) {
            this.fetchCharacters(response.data.info.next);
            console.log(response.data.results)
          }
          
        })
        .catch(error => {
          console.error('Error al obtener la lista de personajes:', error);
        });
    },
  },
};
</script>

<template>
    <section>
      <h1>Lista de personajes</h1>
      <div class="filter">
        <div class="item">All</div>
        <div class="item">Alive</div>
        <div class="item">Ded</div>
        <div class="item">Unknown</div>
      </div>
  
      <div v-if="characters.length > 0" class="container-cards">
        <ul class="card-list">
          <li v-for="character in characters" :key="character.id" class="card">
            <div class="image">
                <img :src="character.image" :alt="character.name">
            </div>
            
            <h2>{{ character.name }}</h2>
            <p>Specie: {{ character.species }}</p>
            <p>Status: {{ character.status }}</p>
            <p>Gender: {{ character.gender }}</p>
            <!-- Otros detalles del personaje según tu necesidad -->
          </li>
        </ul>
      </div>
    </section>
  </template>
  
  <style scoped>
    section {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      margin: 0rem 10rem 0rem 10rem;
    }
  
    section > h1 {
      text-align: center;
      font-size: 40px;
      margin-top: 4rem;
    }
    section .filter{
        width: 20%;
        display: flex;
        justify-content: space-around;
        align-items: center;
        background-color: var(--background-card);
        padding: 0.9rem;
        font-size: 15   px;
        border-radius: 0.5rem;
        cursor: pointer;
    }
    section .filter :hover{
        color: var(--text-orange);
    }
  
    section .container-cards {
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  
    .card-list {
      list-style: none;
      padding: 0;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
    }
  
    .card {
      width: 18%;
      
      margin: 1%;
      border-radius: 2rem;
      background-color: var(--background-card);
      
      box-sizing: border-box;
      text-align: center;
      overflow: hidden;
    }
  
    .card .image {
      width: 100%;
    }
    .card .image > img {
      width: 100%;
      
      
    }
  </style>
  

  
  