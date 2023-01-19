<template>
  <div>
    <!-- <NuxtWelcome /> -->
    <div class="container">
      <!-- <h2>Get a Pokemon!</h2> -->
      <button class="btnGetPokemon" @click="refresh" :disabled="pending">
        Get a Pokemon!
       
        <!-- <span v-if="pending">Loading..</span>
        <span v-else>Get a Pokemon!</span> -->
      </button>

      <div class="pokemon">
        <div v-if="pending">
          <div class="lds-ring"><div></div><div></div><div></div><div></div></div>
        </div>
        <div v-else-if="error || !pokemonData">
          <h2>Ha ocurrido un error</h2>
        </div>
        <div v-else>
          <div class="name">{{ pokemonData.name }}</div>
          <p class="number">{{ pokemonNumber }}</p>
          <ul>
            <li v-for="(type, index) in pokemonData.types" :key="index" :class="type.type.name">{{ type.type.name }}
            </li>
          </ul>
          <img
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${pokemonData.id}.png`"
            alt="">
        </div>
      </div>
    </div>

  </div>
</template>
<script setup>

const { data: pokemonData, pending, refresh, error } = await useAsyncData(
  'pokemonDatas',
  async () => {
    const randomNumber = await Math.floor(Math.random() * 900);
    return $fetch(`https://pokeapi.co/api/v2/pokemon/${randomNumber}`)
  },
  { pick: ['name', 'id', 'types'] }
)

const pokemonNumber = computed(() => {
  return '# ' + pokemonData.value.id.toString().padStart(3, '0')
})
</script>

<style>
.btnGetPokemon {
  background-color: rgb(74, 137, 74);
  padding: 1em 2em;
  margin: 3em 0;
  border-radius: 5px;
  cursor: pointer;
  border: none;
  box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;
  transition: .2s linear;
}

.btnGetPokemon:active {
  box-shadow: none;
  background-color: rgb(67, 110, 67);
}
.btnGetPokemon:disabled{
  background-color: rgb(129, 174, 129);
  user-select: none;
}
.pokemon .name {
  text-transform: uppercase;
  font-size: 3em;
  font-weight: bolder;
}

.pokemon ul {
  margin: 1em 0;
}

.pokemon li {
  display: inline;
  border-radius: 5px;
  padding: 5px;
}

.pokemon li+li {
  margin-left: 10px;
}

.pokemon img {
  display: block;
  width: 100%;
}
</style>