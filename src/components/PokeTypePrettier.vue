<template lang="pug">
  .badge(:style="`background: ${types_colors[pokemon_type]}`") {{ pokemon_type }}
</template>

<script>

export default {
  name: 'PokeTypePrettier',
  props: {
    pokemon_type: String
  },
  data() {
    return {
      types_colors: {
        normal: '#a8a878',
        fighting: '#c03028',
        flying: '#a890f0',
        poison: '#a040a0',
        ground: '#e0c068',
        rock: '#b8a038',
        bug: '#a8b820',
        ghost: '#705898',
        fire: '#f08030',
        water: '#6890f0',
        grass: '#78c850',
        electric: '#f8d030',
        psychic: '#f85888',
        ice: '#98d8d8',
        dragon: '#7038f8',
        fairy: '#ee99ac',
        steel: '#b8b8d0'
      }
    }
  },
}
</script>

<style <style lang="sass" scoped>
  .badge
    width: 55px
    padding: 5px
    margin: 5px
</style>
