<template>
  <v-col class="d-flex justify-center">
    <section v-if="errored">
      <v-alert type="error">{{errmsg}}</v-alert>
    </section>
    <section v-else >
      <div v-if="loading">
        <v-card outlined class="card-loading text-center">
          <v-progress-circular
            indeterminate
            size="70"
            color="primary"
          ></v-progress-circular>
          <div>Loading...</div>
        </v-card>
      </div>
      <router-link v-else v-bind:to="{path:'/pokemon/'+info.id, query:{locale: this.locale}}">

      <v-card outlined hover class="text-center pa-1">
        <div class ="img-blank">
          <v-img
            max-width="200"
            max-height="200"
            :src="this.pic"
            :alt="pokemon.name"
          >
          </v-img>
        </div>
        <v-chip>No.{{info.id}}</v-chip>
        <span class="text-body1">{{this.jpName}}</span>
      </v-card>

      </router-link>
    </section>
  </v-col>
</template>

<script>
export default {
  name: 'Pokemontile',
  props: ['pokemon', 'locale'],
  data: function () {
    return {
      errored: false,
      errmsg: '',
      loading: true,
      info: '',
      jpName: '',
      pic: ''
    }
  },
  async mounted (){
    this.info = await this.getPokeInfo(this.pokemon.url);
    this.pic = this.info.sprites.other["official-artwork"].front_default;
    this.jpName = await this.getJpString(this.info.species.url);
    this.loading = false
  },
  methods : {
    async getPokeInfo(url) {
      let res = null;
      res = await this.$axios.get(url)
      .catch(error => {
        this.errmsg = error;
        this.errored = true;
      })
      return res.data;
    },
    async getJpString (url) {
      let res = null;
      res = await this.$axios.get(url)
      .catch(error => {
        this.errmsg = error;
        this.errored = true;
      })
      let ret = res.data.names.find((item) => {
        if (item.language.name === this.locale) return true;
      })
      return ret.name;
    }
  }
}
</script>

<style scoped>
.card-loading {
  width: 210px;
  height: 252px;
  padding-top:80px;
}

.img-blank {
  width: 200px;
  height: 200px;
}

a {text-decoration: none;}
</style>