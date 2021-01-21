<template>
  <div class="tile">
    <section v-if="errored">
      <p>{{errmsg}}</p>
    </section>
    <section v-else >
      <div v-if="loading">Loading...</div>
      <div>
        <input type="button" value="back" @click="backHome()">
      </div>
      <div>
        <img v-bind:src="this.pic" v-bind:alt="this.jpName">
      </div>
      <span>No.{{info.id}}: </span>
      <span>{{this.jpName}}</span>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Pokemon',
  data: function () {
    return {
      errored: false,
      errmsg: '',
      loading: true,
      baseurl: 'https://pokeapi.co/api/v2/pokemon/',
      info: '',
      jpName: '',
      pic: ''
    }
  },
  mounted (){
    this.$axios
      .get(this.baseurl + this.$route.params.id)
      .then((res) => {
        this.info = res.data;
        this.pic = res.data.sprites.other["official-artwork"].front_default;
      })
      .then(() => {
        this.$axios
        .get(this.info.species.url)
        .then((res) => {
          this.jpName = this.getJpName(res.data.names);
        })
      })
      .catch(error => {
        this.errmsg = error;
        this.errored = true
      })
      .finally(() => this.loading = false)
  },
  methods : {
    getJpName (names) {
      const obj = names.find((item) => {
        if (item.language.name === "ja-Hrkt") return true;
      })
      return obj.name;
    },
    backHome () {
      this.$router.push('/')
      return;
    }
  }
}
</script>

<style>

</style>