<template>
  <div >
    <MyHeader></MyHeader>
    <section v-if="errored">
      <p>{{errmsg}}</p>
    </section>
    <section v-else class="content">
      <div v-if="loading">Loading...</div>
      <div v-else>
        <div>
          <span class="pad"></span>
          <input type="button" value="Prev" @click="getNewPage(prev)">
          <input type="button" value="Next" @click="getNewPage(next)">
          <div class="box">
            <PokemonTile
              v-for="pokemon in results"
              v-bind:key="pokemon.url"
              v-bind:pokemon="pokemon"
            ></PokemonTile>
          </div>
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
      next: 'https://pokeapi.co/api/v2/pokemon/?offset=0&limit=24',
      prev: ''

    }
  },
  mounted () {
    this.getNewPage(this.next);
  },
  methods: {
    getNewPage : function (url) {
      if (url === null)
        return ;
      this.loading = true;
      this.$axios
      .get(url)
      .then((res) => {
        this.results = res.data.results;
        this.next = res.data.next;
        this.prev = res.data.previous;
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
.pad {
  margin-right: 90%;
}
.content {
  width: 100%;
  height: 100%;
  background-color: #b5c6e7;
}

.box {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  width: 100%;
}
</style>