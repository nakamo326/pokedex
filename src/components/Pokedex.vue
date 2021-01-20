<template>
  <div >
    <section v-if="errored">
    <p>{{errmsg}}</p>
    </section>
    <section v-else class="content">
      <div v-if="loading">Loading...</div>
      <div v-else>
        <h2>Pokedex.vue</h2>
        <div class="box">
          <PokemonTile
            v-for="pokemon in results"
            v-bind:key="pokemon.url"
            v-bind:pokemon="pokemon"
          ></PokemonTile>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import PokemonTile from './pokemontile.vue'

export default {
  name: 'Pokedex',
  components: {
    PokemonTile
  },
  data: function () {
    return {
      errored: false,
      errmsg: '',
      loading: true,
      results: ''
    }
  },
  mounted () {
    this.$axios
      .get('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=50')
      .then((res) => {
        this.results = res.data.results;
      })
      .catch(error => {
        this.errmsg = error;
        this.errored = true;
      })
      .finally(() => this.loading = false)
  }
}
</script>

<style>
.content {
  width: 100%;
  height: 100%;
  background-color: aquamarine;
}

.box {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  width: 100%;
  box-sizing: border-box;
}
</style>