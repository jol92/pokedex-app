<template lang="pug">
    b-card.card(
      v-if="!loading && pokemon_data && pokemon_data.sprites"
      v-on:mouseover="show_info = true"
      v-on:mouseleave="show_info = false"
      :title="!show_info ? createPokemonTitle(pokemon_data.name) : null"
      :sub-title="!show_info ? `#${pokemon_data.id}` : null"
      :img-src="!show_info ? pokemon_data.sprites.front_default : null"
      tag="article"
      class="mb-2"
    )
      b-card-text.card-text(v-if="!show_info")
        PokeTypePrettier(v-for="type in pokemon_data.types" :key="type.slot" :pokemon_type="type.type.name")
      b-card-text.card-text.card-with-background(v-else)
        .stat
          .title Name:
          .value {{ createPokemonTitle(pokemon_data.name) }}
        .stat
          .title Weight:
          .value {{ pokemon_data.weight }}
        .stat
          .title Height:
          .value {{ pokemon_data.height }}
        .stat
          .title Base exp:
          .value {{ pokemon_data.base_experience }}
        .stat(v-for="stat in pokemon_data.stats")
          .title {{ removeHyphenFromString(stat.stat.name) }}:
          .value {{ stat.base_stat }}
    b-card.card(v-else)
      b-card-text
        p Loading...
        b-spinner()
</template>

<script>
import axios from "axios"
import PokeTypePrettier from './PokeTypePrettier.vue'


export default {
  name: 'PokemonCard',
  props: {
    pokemon_url: String
  },
  components: {
    PokeTypePrettier
  },
  mounted() {
    this.getPokemonDataByUrl()
  },
  data() {
    return {
      show_info: false,
      pokemon_data: {},
      loading: false
    }
  },
  methods: {
    async getPokemonDataByUrl() {
      this.loading = true
      const {data} = await axios.get(this.pokemon_url)
      this.loading = false
      if(data) this.pokemon_data = data
    },
    removeHyphenFromString(string) {
      // Some stats like special attack comes with hyphens so i remove from the string
      return string.replace(/-/g, " ")
    },
    createPokemonTitle(pokemon_name) {
      // The pokemon name comes without capitalize
      return  `${pokemon_name.charAt(0).toUpperCase() + pokemon_name.slice(1)}`
    }
  },
}
</script>

<style <style lang="sass" scoped>
  .card
    width: 230px
    height: 350px
    padding: 10px
    margin: 10px
    // cursor: pointer // TODO: Click in a Pokemon to display a modal with more details, also add styles -shadows-
    &:hover
      .card-text
        opacity: 1
        animation-name: fadeInOpacity
        animation-iteration-count: 1
        animation-timing-function: ease-in
        animation-duration: 0.7s
    .stat
      margin-top: 5px
      display: flex
      .title
        font-weight: bold
      .value
        margin-left: 5px
    @keyframes fadeInOpacity
      0%
        opacity: 0.3
      100%
        opacity: 1
</style>
