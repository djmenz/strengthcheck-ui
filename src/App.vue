<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
    <h1>StrengthCheck.me</h1>
      </div>
      <v-spacer></v-spacer>

      <v-btn
        href="/about"
        target="_blank"
        text
      >
        <span class="mr-2">F.A.Q</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>

 <v-footer
    color="primary lighten-1"
    padless
  >
    <v-row
      justify="center"
      no-gutters
    >
      <v-btn
        v-for="link in links"
        :key="link"
        color="white"
        text
        rounded
        class="my-2"
      >
        {{ link }}
      </v-btn>
      <v-col
        class="primary lighten-2 py-4 text-center white--text"
        cols="8"
      >
      <strong>This page relies on data sourced from the OpenPowerlifting project - <a href="https://www.openpowerlifting.org">https://www.openpowerlifting.org</a></strong>
      </v-col>
            <v-col
        class="primary lighten-2 py-4 text-right white--text"
        cols="4"
      >
 Contact - daniel.j.menz@gmail.com
            </v-col>
    </v-row>
  </v-footer>

  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
