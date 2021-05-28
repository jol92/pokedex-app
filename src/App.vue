<template lang="pug">
  div(id="app")
    .container
      .logo
        img.desktop(src="./assets/logo-desktop.png" width="750px")
        img.mobile(src="./assets/logo-mobile.png" width="250")
      .filter-box
        b-input-group.search(size="sm" class="mb-2")
          b-input-group-prepend(is-text)
            b-icon(icon="search")
          b-form-input(v-model="filters.pokemon_name" placeholder="Search a pokemon" @input="searchKantoPokemon")
      .pokemon-card-box(v-if="displayedPokemons.length > 0")
        PokemonCard(:pokemon_url="pokemon.url" v-for="pokemon in displayedPokemons" :key="pokemon.url")
      .no-pokemon-box(v-else)
        .title No Pokémon matched your search.
      .pagination-box
        pagination(
          :records="pokemon_list.length"
          v-model="pagination.page"
          :per-page="pagination.per_page"
          :options="{chunksNavigation: 'fixed', chunk: 5, texts: {count: '', first: '', last: ''} }")
</template>

<script>
import PokemonCard from './components/PokemonCard.vue'
import axios from "axios"

export default {
  name: 'App',
  components: {
    PokemonCard
  },
  mounted() {
    this.getKantoPokedex()
  },
  data() {
    return {
      current_pokemon: {},
      pokemon_complete_list: [],
      pokemon_list: [],
      filters: {
        pokemon_name: ''
      },
      pagination: {
        page: 1,
        per_page: 9
      }
    }
  },
  computed: {
    displayedPokemons() {
      // The pokeApi dont have a call where you can choose a region to paginate so i did the pagination in client side
      const startIndex = this.pagination.per_page * (this.pagination.page - 1);
      const endIndex = startIndex + this.pagination.per_page;
      let new_array = this.pokemon_list.slice(startIndex, endIndex)
      return new_array
    }
  },
  methods: {
    async getKantoPokedex() {
      try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?limit=151`)
        this.pokemon_complete_list = response.data.results
        this.pokemon_list = response.data.results
      } catch (err) {
        if (err.response) {
          console.log("Server Error:", err)
        } else if (err.request) {
          console.log("Network Error:", err)
        } else {
          console.log("Client Error:", err)
        }
      }
    },
    searchKantoPokemon() {
      // I didnt found in PokeApi a way to filter Pokemons by only few letters and not entery name, so i filter by myself in client side
      this.pagination.page = 1
      let new_array = []
      this.pokemon_complete_list.forEach(el => {
        if (el.name.includes(this.filters.pokemon_name)) new_array.push(el)
      })
      this.pokemon_list = new_array
    }
  }
}
</script>

<style <style lang="sass">
  #app
    background: #F2F2F2
    height: 100%
    min-height: 100vh
    font-family: Avenir, Helvetica, Arial, sans-serif
    -webkit-font-smoothing: antialiased
    -moz-osx-font-smoothing: grayscale
    text-align: center
    color: #2c3e50
    padding: 5%
    .container
      width: 100%
      display: flex
      justify-content: center
      flex-wrap: wrap
      .filter-box
        width: 100%
        padding: 10px
        display: flex
        justify-content: center
        .search
          margin: 10px 0px
          min-width: 300px
          max-width: 550px
      .pokemon-card-box
        display: flex
        flex-wrap: wrap
        max-width: 800px
        padding: 10px
        justify-content: center
      .no-pokemon-box
        font-weight: 600
        font-size: 23px
        color: #d3363f
      .pagination-box
        margin: 10px
        display: flex
        justify-content: center
        width: 100%
  /* when screen is less than 768px wide
    show mobile version and hide desktop */
  @media (max-width: 768px)
    .logo
      .mobile
        display: block
      .desktop
        display: none
        width: 100%
  @media (min-width: 600px)
    .logo
      .mobile
        display: none
        width: 100%
      .desktop
        display: block
</style>>
