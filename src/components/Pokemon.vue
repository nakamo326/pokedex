<template>
  <v-card class="text-center">
    <section v-if="errored">
      <v-alert type="error">{{errmsg}}</v-alert>
    </section>
    <section v-else >
      <div v-if="loading">
        <v-chip label style="margin:0px">Loading...</v-chip>
      </div>
      <div v-else>
        <div>
          <v-btn color="primary" dark absolute left @click="backHome()">back</v-btn>
        </div>
        <div>
          <v-chip color="orange">No.{{info.id}}</v-chip>
          <br />
          <span class="text-h3">{{this.pokeName}}</span>
          <br />
          <img v-bind:src="this.pic" v-bind:alt="this.pokeName">
        </div>

        <pokeType
          v-if="jpType0!=''"
          v-bind:typeText="jpType0"
          v-bind:url="typeUrl0"
        ></pokeType>
        <pokeType
          v-if="jpType1!=''"
          v-bind:typeText="jpType1"
          v-bind:url="typeUrl1"
        ></pokeType>
        <v-simple-table dense>
            <tbody>
              <tr v-for="(title, index) in this.stats_title" :key="index">
                <td>{{title}}</td>
                <td>{{info.stats[stats_title.indexOf(title)].base_stat}}</td>
              </tr>
            </tbody>
        </v-simple-table>

      </div>
    </section>
  </v-card>
</template>

<script>
import pokeType from './pokeType.vue'

export default {
  name: 'Pokemon',
  components: {
    pokeType
  },
  data: function () {
    return {
      errored: false,
      errmsg: '',
      loading: true,
      info: '',
      pic: '',
      pokeName: '',
      jpType0: '',
      typeUrl0: '',
      jpType1: '',
      typeUrl1: '',
      stats_title: []
    }
  },
  async mounted (){
    this.info = await this.getPokeInfo(this.$route.params.id);
    this.pic = this.info.sprites.other["official-artwork"].front_default;
    this.typeUrl0 = this.info.types["0"].type.url
    this.jpType0 = await this.getJpString(this.typeUrl0);
    if (Object.keys(this.info.types).length == 2)
    {
      this.typeUrl1 = this.info.types["1"].type.url;
      this.jpType1 = await this.getJpString(this.typeUrl1);
    }
    Promise.all([
      this.pokeName = await this.getJpString(this.info.species.url),
      this.stats_title[0] = await this.getJpString(this.info.stats["0"].stat.url),
      this.stats_title[1] = await this.getJpString(this.info.stats["1"].stat.url),
      this.stats_title[2] = await this.getJpString(this.info.stats["2"].stat.url),
      this.stats_title[3] = await this.getJpString(this.info.stats["3"].stat.url),
      this.stats_title[4] = await this.getJpString(this.info.stats["4"].stat.url),
      this.stats_title[5] = await this.getJpString(this.info.stats["5"].stat.url),
    ])
    .catch(error => {
      this.errmsg = error;
      this.errored = true;
    })
    this.loading = false
  },
  methods : {
    async getPokeInfo (id) {
      let res = null;
      res = await this.$axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
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
        if (item.language.name === this.$route.query.locale) return true;
      })
      return ret.name;
    },
    backHome () {
      this.$router.push('../')
    }
  }
}
</script>

<style>
.v-chip {
  margin: 0px 5px 10px;
}
</style>