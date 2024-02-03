<script>
import axios from 'axios';

export default {
  data() {
    return {
      characters: [],
      filteredCharacters: [],
      searchTerm: '',
      activeFilter: 'All',
    };
  },
  mounted() {
    this.fetchCharacters('https://rickandmortyapi.com/api/character/')

    .then(() => {
      this.filteredCharacters = this.characters;
    });
  },
  methods: {
    fetchCharacters(url) {
  return new Promise((resolve, reject) => {
    axios.get(url)
      .then(response => {
        this.characters = this.characters.concat(response.data.results);

        
        if (response.data.info.next) {
          this.fetchCharacters(response.data.info.next).then(resolve);
        } else {
          resolve();
        }
      })
      .catch(error => {
        console.error('Error al obtener la lista de personajes:', error);
        reject(error);
      });
  });
},

  filterByStatus(status) {
  const lowerCaseStatus = status.toLowerCase();
  this.activeFilter = status;
  if (lowerCaseStatus === 'all') {
    this.filteredCharacters = this.characters;
  } else {
    this.filteredCharacters = this.characters.filter(character => character.status.toLowerCase() === lowerCaseStatus);
  }
},

    filterByName() {
      const searchTerm = this.searchTerm.toLowerCase();
      this.filteredCharacters = this.characters.filter(
        character => character.name.toLowerCase().includes(searchTerm)
      );
    },
  },
};
</script>

<template>
    <section>
      <h1>Rick and morty <br> characters</h1>
      
      <div class="filter">
        <div class="item" :class="{ active: activeFilter === 'All' }" @click="filterByStatus('All')">All</div>
        <div class="item" :class="{ active: activeFilter === 'Alive' }" @click="filterByStatus('Alive')">Alive</div>
        <div class="item" :class="{ active: activeFilter === 'Dead' }" @click="filterByStatus('Dead')">Dead</div>
        <div class="item" :class="{ active: activeFilter === 'Unknown' }" @click="filterByStatus('Unknown')">Unknown</div>
      </div>

      
      <input type="text" placeholder="Search by name" v-model="searchTerm" @input="filterByName">
      
      <div v-if="filteredCharacters.length > 0" class="container-cards">
        <ul class="card-list">
          <li v-for="character in filteredCharacters" :key="character.id" class="card">

            
            <div class="image">
                <img :src="character.image" :alt="character.name">
            </div>
            
            
            <h2>{{ character.name }}</h2>
            
            
            <div class="status">
              <span 
              :class="
                character.status == 'Alive' ? 'alive' :
                character.status == 'Dead' ? 'dead' :
                'unknown'">
              </span>
              <span>{{ character.status }} - {{ character.species }}</span>
            </div>

            
            <div class="information">
              <span>Gender: {{ character.gender }}</span>
              <span>Origin: {{ character.origin.name }}</span>
              <span>Location: {{ character.location.name }}</span>
            </div>
            
            
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
      font-size: 45px;
      margin-top: 4rem;
    }
    section .filter{
        width: 25%;
        display: flex;
        justify-content: space-around;
        align-items: center;
        background-color: var(--background-card);
        padding: 0.9rem;
        font-size: 15   px;
        border-radius: 0.5rem;
        cursor: pointer;
        margin-bottom: 1rem;
    }
    section .filter :hover{
        color: var(--text-orange);
    }

    section > input{
      width: 25%;
      padding: 0.9rem;
      border-radius: 0.5rem;
      border: none;
      margin-bottom: 1rem;
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
      width: 20%;
      margin: 1%;
      border-radius: 1rem;
      background-color: var(--background-card);
      box-sizing: border-box;
      text-align: center;
      overflow: hidden;
      transition: transform 200ms ease-in-out;
      cursor: pointer;
    }
    .card:hover{
      transform: scale(1.05);
      h2{
        color: var(--text-orange);
      }
    }
    .card .image {
      width: 100%;
    }
    .card .image > img {
      width: 100%;
    }
    .card > h2{
      margin: 5px;
    }

    .status {
      margin: 5px;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: row;
    }
    .status span{
      color: var(--text-gray);
      
    }
    .status span:first-child{
      width: 10px;
      height: 10px;
      border-radius: 50%;
      margin-right: 0.5rem;
    }
    .alive {
        background-color: green;
      }
      .dead {
        background-color: red;
      }
      .unknown {
        background-color: white;
      }
    
    .card .information{
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
    }
    .card .information > span{
      color: var(--text-gray);
      margin: 2px;
    }
    .card .information > span:last-child{
      margin-bottom: 20px;
    }
    .item.active {
      color: orange;
    }

    
    /* MOBILE */
    @media only screen and (min-width: 370px) and (max-width: 600px) {
      section{
        width: 100%;
        margin: 0;
      }
      section > h1{
        font-size: 35px;
        margin-top: 1rem;
      }
      section .filter{
        width: 80%;
      }
      section > input{
        width: 80%;
      }
      .card{
        width: 80%;
        margin-bottom: 1rem;
      }
    }

    /* TABLET*/
    @media only screen and (min-width: 601px) and (max-width: 768px) {
      section{
        width: 100%;
        margin: 0;
      }
      section > h1{
        font-size: 35px;
        margin-top: 1rem;
      }
      section .filter{
        width: 86%;
      }
      section > input{
        width: 86%;
      }
      .card{
        width: 40%;
        margin-bottom: 1rem;
      }
    }


    /* LAPTOP */
    @media only screen and (min-width: 769px) and (max-width: 1024px) {
      section{
        width: 100%;
        margin: 0;
      }
      section > h1{
        font-size: 45px;
        margin-top: 4rem;
      }
      section .filter{
        width: 40%;
      }
      section > input{
        width: 40%;
      }
      .card{
        width: 30%;
        margin-bottom: 1rem;
      }
    }


    /* LARGE SCREEN */
    @media only screen and (min-width: 1025px) and (max-width: 1201px) {
      section{
        width: 100%;
        margin: 0;
      }
      section > h1{
        font-size: 45px;
        margin-top: 4rem;
      }
      section .filter{
        width: 45%;
      }
      section > input{
        width: 45%;
      }
      .card{
        width: 20%;
        margin-bottom: 1rem;
      }
    }
    
  </style>
  

  
  