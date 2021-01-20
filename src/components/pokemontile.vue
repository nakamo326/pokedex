<template>
  <div class="tile">
    <section v-if="errored">
    <p>{{errmsg}}</p>
    </section>
    <section v-else >
      <div v-if="loading">Loading...</div>
      <div v-else >
        <img v-bind:src="this.pic" alt="sprite" width="200">
      </div>
      <span>No.{{info.id}}: </span>
      <span>{{this.jpName}}</span>
    </section>
  </div>
</template>

<script>
export default {
  name: 'pokemontile',
  props: ['pokemon'],
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
  mounted (){
    this.$axios
      .get(this.pokemon.url)
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
    }
  }
}
</script>

<style scoped>
.tile {
  text-align: center;
  margin: 5px 5px 5px 5px;
  background-color:antiquewhite;
  border-radius: 10px;
}

</style>