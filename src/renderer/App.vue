<template>
  <v-app id="inspire" dark>
    <v-content>
      <v-container fluid>
        <!--<v-container fluid fill-height>-->

        <router-view/>

        <!--<v-layout justify-center align-center>-->
        <!--<v-flex shrink>-->
        <!--<v-tooltip right>-->
        <!--<v-btn-->
        <!--slot="activator"-->
        <!--:href="source"-->
        <!--icon-->
        <!--large-->
        <!--target="_blank"-->
        <!--&gt;-->
        <!--<v-icon large>code</v-icon>-->
        <!--</v-btn>-->
        <!--<span>Source</span>-->
        <!--</v-tooltip>-->
        <!--<v-tooltip right>-->
        <!--<v-btn slot="activator" icon large href="https://codepen.io/johnjleider/pen/qxQWda" target="_blank">-->
        <!--<v-icon large>mdi-codepen</v-icon>-->
        <!--</v-btn>-->
        <!--<span>Codepen</span>-->
        <!--</v-tooltip>-->
        <!--</v-flex>-->
        <!--</v-layout>-->
      </v-container>
    </v-content>
    <v-footer app fixed>
      <span>&copy; 2018 Component Team - Love code Love Olga Buzova</span>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    name: 'dashboard'
  }
</script>

<style>
  /* CSS */
</style>
