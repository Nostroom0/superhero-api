<template>
  <h1>Welcome in the Super Heroes Data Base</h1>
  <input @input="updateSuperheroName" v-model="superheroName" type="search">
  <!-- <button @click="getSuperheroes">Search</button> -->
  <div class="content" v-if="filteredSuperheroes.length > 0">
    <div @click="displays='appear', flex='extend'" :class="flex"  v-for="hero in filteredSuperheroes" :key="hero.name">
      <h2>{{ hero.name }}</h2>
      <img :src="hero.images.md" :alt="hero.name">
      <div >
        <div :class="displays">
          <div class="biography">
            <h3>Biography</h3>
            <p>nom complet: {{ hero.biography.fullName }}</p>
            <p>alteregos: {{ hero.biography.alterEgos }}</p>
            <p>Aliases : {{ hero.biography.aliases }}</p>
            <p> Place of Birth : {{ hero.biography.placeOfBirth }}</p>
            <p> First appearance : {{ hero.biography.firstAppearance }}</p>
            <p>Publisher : {{ hero.biography.publisher }}</p>
            <p>{{ hero.biography.alignment }}</p>
          </div>
          <div class="work">
            <h3>Work</h3>
            <p>Work : {{ hero.work.occupation }}</p>
            <p>Base / QG : {{ hero.work.base }}</p>
          </div>
          <div class="connections">
            <h3>Connections</h3>
            <p>Work : {{ hero.connections.groupAffiliation }}</p>
            <p>Base / QG : {{ hero.connections.relatives }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <p>No superheroes found.</p>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const superheroName = ref('');
const superheroes = ref([]);
const filteredSuperheroes = ref([]);

const getSuperheroes = () => {
  fetch(`https://akabab.github.io/superhero-api/api/all.json`)
    .then(response => response.json())
    .then(data => {
      superheroes.value = data;
      filterSuperheroes();
    })
    .catch(error => {
      console.error("Erreur lors de la récupération des données", error);
    });
};

const filterSuperheroes = () => {
  if (superheroName.value === '') {
    filteredSuperheroes.value = [];
  } else {
    filteredSuperheroes.value = superheroes.value.filter((hero: { name: string }) => {
      return hero.name.toLowerCase().startsWith(superheroName.value.toLowerCase());
    });
  }
};

const updateSuperheroName = () => {
  filterSuperheroes();
};



const displays = ref('display')
const flex = ref('flex')

onMounted(getSuperheroes);

</script>

<style scoped>
/* Votre style CSS personnalisé ici */
  .content{
    display: grid;
  }
  .flex{
    position: relative;
    height: 200px;
    width: 100%;
    display: flex;
    flex-direction: column;
  }
  img{
    height: 100%;
    width: 100%;
    object-position: 0 25%;
    object-fit: cover;
    filter: brightness(.7);
  }
  h2{
    margin: 0;
    position: absolute;
    top: 50%;
    left: 20%;
    transform: translate(-50%, -50%);
    z-index: 10;
    color: #fff;
    text-transform: uppercase;
  }
  .display{
    display: none;
  }
  .appear{
    display: block;
  }
  .extend{
    height: 100vh;
  }
  .expanded-content {
  height: auto;
}
</style>
