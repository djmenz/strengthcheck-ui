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
<div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="purple"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Contact
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Questions
        </v-card-title>

        <v-card-text>
          For any questions/suggestions, please contact via email - daniel.j.menz@gmail.com
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
      <!-- <v-btn
        href="/about"
        target="_blank"
        text
      >
        <span class="mr-2">F.A.Q</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn> -->
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

      <v-col
        class="primary lighten-2 py-4 text-center white--text"
        cols="12"
      >
      <strong>This page relies on data sourced from the OpenPowerlifting project - <a href="https://www.openpowerlifting.org">https://www.openpowerlifting.org</a></strong>
            </v-col>
    </v-row>
  </v-footer>

  </v-app>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      dialog: false,
    };
  },
};
</script>
