<template>
  <div>
    <h1>Pokedex.vue</h1>
    <h2>
      {{count}}
    </h2>
    <div>{{list}}</div>
  </div>
</template>

<script>
export default {
  name: 'Pokedex',
  data: function () {
    return {
      count: '',
      list: {}
    }
  },
  mounted () {
    this.$axios
      .get('https://pokeapi.co/api/v2/pokemon/')
      .then((res) => {
        this.count = res.data.count;
      })
  }
}
</script>

<style>

</style>