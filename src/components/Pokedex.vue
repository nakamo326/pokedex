<template>
  <div >
    <MyHeader></MyHeader>
    <section v-if="errored">
      <p>{{errmsg}}</p>
    </section>
    <section v-else class="content">
      <div v-if="loading">Loading...</div>
      <div v-else>
        <input type="button" value="Next" @click="getNextPage()">
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
import PokemonTile from './Pokemontile.vue'
import MyHeader from './MyHeader.vue'

export default {
  name: 'Pokedex',
  components: {
    PokemonTile,
    MyHeader
  },
  data: function () {
    return {
      errored: false,
      errmsg: '',
      loading: true,
      results: '',
      next: ''
    }
  },
  mounted () {
    this.$axios
      .get('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=50')
      .then((res) => {
        this.results = res.data.results;
        this.next = res.data.next;
      })
      .catch(error => {
        this.errmsg = error;
        this.errored = true;
      })
      .finally(() => this.loading = false)
  },
  methods: {
    getNextPage : function () {
      this.loading = true;
      this.$axios
      .get(this.next)
      .then((res) => {
        this.results = res.data.results;
        this.next = res.data.next;
      })
      .catch(error => {
        this.errmsg = error;
        this.errored = true;
      })
      .finally(() => this.loading = false)
    }
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