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
          F.A.Q
        </v-btn>
      </template>
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
         F.A.Q
        </v-card-title>

        <v-card-text>

        <div>
          <strong>Where does this data come from?</strong>
          </div>
          <div>
           This page relies on data sourced from the (awesome) OpenPowerlifting project -
           <a href="https://www.openpowerlifting.org">https://www.openpowerlifting.org</a>, this can be downloaded
           from  <a href="https://openpowerlifting.gitlab.io/opl-csv/">https://openpowerlifting.gitlab.io/opl-csv/</a>
        </div>
        <br>

          <div>
          <strong>What exact data is presented?</strong>
          </div>
          <div>
            The drug tested results come from all IPF results. All Results shows all competitions,
             tested and untested.
            Note that Bench and Deadlift results include all results from single lift/push pull
             competitions,
             where as Squat and Total
            are only from full 3 lift competition results. (I may put this as a toggle
            option later)
        </div>
        <br>

         <div>
          <strong>Can you give results for only some age groups?</strong>
          </div>
          <div>
            Not currently but this feature may be added (Let me know if you
            would find this useful.)
        </div>
        <br>

          <div>
          <strong>For any questions/suggestions</strong>
          </div>
          <div>
            Please contact via email - daniel.j.menz@gmail.com
        </div>
        <br>

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
