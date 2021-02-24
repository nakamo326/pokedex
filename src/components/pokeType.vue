<template>
  <v-chip :color="setColor(typeId)" text-color="white">
    {{this.typeText}}
  </v-chip>
</template>

<script>
export default {
  name: 'pokeType',
  props: ['url', 'typeText'],
  data () {
    return {
      typeId: '',
      color:{
        1: 'brown',
        2: 'red darken-3',
        3: 'deep-purple lighten-3',
        4: 'purple darken-1',
        5: 'lime darken-4',
        6: 'brown lighten-2',
        7: 'light-green darken-1',
        8: 'deep-purple lighten-2',
        9: 'blue-grey lighten-3',
        10: 'deep-orange darken-1',
        11: 'light-blue lighten-2',
        12: 'green',
        13: 'yellow',
        14: 'pink lighten-2',
        15: 'cyan lighten-3',
        16: 'deep-purple accent-2',
        17: 'brown lighten-3',
        18: 'pink lighten-4',
        19: 'teal lighten-3',
        20: 'blue-grey darken-3'
      }
    }
  },
  async mounted (){
    let res = null;
    res = await this.$axios.get(this.url)
    .catch(error => {
      this.errmsg = error;
      this.errored = true;
    })
    this.typeId = res.data.id;
  },
  methods : {
    setColor (typeId){
      return this.color[typeId];
    }
  }
}
</script>

<style>

</style>