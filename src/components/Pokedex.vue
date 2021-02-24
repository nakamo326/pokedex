<template>
  <div >
    <MyHeader @catchLang='reload($event)' @backhome='setPageNum(1)'></MyHeader>
    <section v-if="errored">
      <v-alert type="error">{{errmsg}}</v-alert>
    </section>
    <section v-else class="primary lighten-3">
      <v-container>
        <div v-if="loading" class="d-flex justify-center">
          <v-progress-circular
          indeterminate
          size="100"
          color="primary"
          ></v-progress-circular>
          <br/>
          <div class="primary--text">Loading...</div>
        </div>
        <v-row v-else>
            <PokemonTile
              v-for="pokemon in results"
              v-bind:key="pokemon.url"
              v-bind:pokemon="pokemon"
              v-bind:locale="locale"
            ></PokemonTile>
        </v-row>
        <div v-if="showModal">
          <div class="modal-route" @click="$router.push('/').catch(()=>{})">
            <div class="modal">
              <router-view></router-view>
            </div>
          </div>
        </div>
        <div class="text-center">
          <v-pagination
            v-model="page"
            :length="pageLength"
            :total-visible="7"
            @input="setPageNum"
          ></v-pagination>
        </div>
      </v-container>
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
  watch: {
    $route: {
      immediate: true,
      handler: function(newVal) {
        this.showModal = newVal.meta && newVal.meta.showModal;
      }
    }
  },
  data: function () {
    return {
      errored: false,
      errmsg: '',
      loading: true,
      showModal: false,
      locale: "ja-Hrkt",
      results: '',
      firstUrl: 'https://pokeapi.co/api/v2/pokemon/?offset=0&limit=30',
      next: '',
      current:'',
      prev: '',
      count: '',
      page: 1,
      perPage: 30,
      pageLength: 0
    }
  },
  mounted () {
    this.getNewPage(this.firstUrl);
  },
  methods: {
    getNewPage (url) {
      if (url === null)
        return ;
      this.loading = true;
      this.$axios
      .get(url)
      .then((res) => {
        this.results = res.data.results;
        this.count = res.data.count;
        this.pageLength = Math.floor(this.count / this.perPage) + 1;
        this.next = res.data.next;
        this.prev = res.data.previous;
        this.current = url;
      })
      .catch(error => {
        this.errmsg = error;
        this.errored = true;
      })
      .finally(() => this.loading = false)
    },
    reload (selectedLanguage) {
      console.log("catch the value is " + selectedLanguage);
      this.locale = selectedLanguage;
      this.getNewPage(this.current);
    },
    setPageNum (pageNumber) {
      this.page = pageNumber;
      let limit = this.perPage;
      let offset = this.perPage * (pageNumber - 1);
      this.getNewPage(`https://pokeapi.co/api/v2/pokemon/?offset=${offset}&limit=${limit}`);
    }
  }
}
</script>

<style>
.modal-route {
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
}

.modal {
  background: #fff;
  border-radius: 4px;
  overflow: hidden;
}
</style>