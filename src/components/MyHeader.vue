<template>
  <v-app-bar app color="primary" dark dense>
    <v-toolbar-title @click="$emit('backhome')"><span class="title">Pokedex.vue</span></v-toolbar-title>

    <v-spacer></v-spacer>

      <!--
      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
      -->

      <v-menu offset-y>
        <template v-slot:activator="{on}">
          <v-btn color="primary" dark v-on="on">Language</v-btn>
        </template>
        <v-list>
          <v-list-item
            @click="$emit('catchLang', item.name)"
            v-for="(item, id) in optionLanguage"
            :key="id"
          >
            <v-list-item-title>{{ item.displayName }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
  </v-app-bar>
</template>

<script>
export default {
  name:'MyHeader',
  data: function (){
    return {
      selectedLanguage: '',
      optionLanguage: [
        { id: 1, displayName: '日本語', name: "ja-Hrkt"},
        { id: 2, displayName: '한국어', name: "ko"},
        { id: 4, displayName: 'Français', name: "fr"},
        { id: 5, displayName: 'Deutsch', name: "de"},
        { id: 6, displayName: 'Español', name: "es"},
        { id: 7, displayName: 'Italian', name: "it"},
        { id: 8, displayName: 'English', name: "en"}
      ]
    }
  }
}
</script>

<style>
.title:hover {
  cursor: pointer;
}
</style>
