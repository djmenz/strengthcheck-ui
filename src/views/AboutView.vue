<template>
  <div class="about">
    <h1>Check how you rank amongst all powerlifting competitors</h1>
      <div id="app">
            <div>
              <v-container fluid>
    <v-row>
      <v-col cols="4" justify="center">
      </v-col>
    <v-col cols="2" justify="center">
        <v-select
              :items="genderSelections"
              v-model="gender"
              item-text="name"
              item-value="id"
              label="Gender"
              :on-change="resetOption()"
            ></v-select>

            <v-select
              :items="testedSelections"
              v-model="testedStatus"
              item-text="name"
              item-value="id"
              label="tested/untested"
              :on-change="resetOption()"
            ></v-select>

            <v-select
              :items="weightClassSelections"
              v-model="weightClass"
              item-text="name"
              item-value="id"
              label="weight class"
              :on-change="showClassData()"
            ></v-select>

            </v-col>
            <v-col cols="2" justify="center">
            <v-text-field
            v-model="formSquat"
            label="Squat"
          ></v-text-field>
            <v-text-field
            v-model="formBench"
            label="Bench"
          ></v-text-field>
            <v-text-field
            v-model="formDeadlift"
            label="Deadlift"
          ></v-text-field>
            <v-text-field
            v-model="formTotal"
            label="Total"
          ></v-text-field>
            </v-col>
            </v-row>
            </v-container>
            </div>
<v-row v-if="showData">
<v-col cols="3" justify="center">
      </v-col>
  <v-col cols="1" justify="center">
    <tr>Results</tr>
    <tr><h2>Pecentile</h2></tr>
     <tr v-for="(item) of classData.Squat" :key="item">{{item[0]}}</tr>
  </v-col>

      <v-col cols="1" justify="center">
        {{resSquat}}%
       <tr><h2>Squat</h2></tr>
        <tr v-for="item of classData.Squat" :key="item">{{item[1]}}</tr>
      </v-col>
      <v-col cols="1" justify="center">
        {{resBench}}%
        <tr><h2>Bench</h2></tr>
        <tr v-for="item of classData.Bench" :key="item"> {{item[1]}}</tr>
      </v-col>
      <v-col cols="1" justify="center">
        {{resDeadlift}}%
        <tr><h2>Deadlift</h2></tr>
        <tr v-for="item of classData.Deadlift" :key="item"> {{item[1]}}</tr>
      </v-col>
      <v-col cols="1" justify="center">
        {{resTotal}}%
        <tr><h2>Total</h2></tr>
        <tr v-for="item of classData.Total" :key="item"> {{item[1]}}</tr>
      </v-col>
        <!-- <ul><li v-for="elem in row" :key="elem">{{elem}}</li></ul> -->
</v-row>
      <!-- {{ data }} -->
  </div>
  </div>
</template>

<script>
import allData from '../../all_data.json';

export default {
  data() {
    return {
      // data: allData,
      classData2: {
        m53raw: {
          Squat: [], Bench: [], Deadlift: [], Total: [],
        },
      },
      genderSelections: ['Male', 'Female'],
      gender: 'Male',
      weightClassAll: {
        MaleTested: ['59', '66', '74', '83', '93', '105', '120', '120+'],
        MaleUntested: ['52', '56', '60', '67.5', '75', '82.5', '90', '100', '110', '125', '140', '140+'],
        FemaleTested: ['43', '47', '52', '57', '63', '72', '84', '84+'],
        FemaleUntested: ['44', '48', '52', '56', '60', '67.5', '75', '82.5', '90', '90+'],
      },
      weightClass: '',
      testedSelections: ['Tested', 'Untested'],
      testedStatus: 'Tested',
      // resSquat: '',
      // resBench: '',
      // resDeadlift: '',
      // resTotal: '55',
      formSquat: '',
      formBench: '',
      formDeadlift: '',
      formTotal: '',
      showData: true,
    };
  },
  computed: {
    classData() {
      if (this.weightClass === '') {
        return [];
      }
      const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}raw`;
      console.log(wClassCode);
      return allData[wClassCode];
    },
    selectedClass() {
      const wclass = `${this.gender}${this.testedStatus}`;
      console.log(wclass);
      return wclass;
    },
    weightClassSelections() {
      return this.weightClassAll[this.selectedClass];
    },
    resSquat() {
      // const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}raw`].Squat;
      const res = [[0, 1]];
      const filtered = res.filter((oneDay) => oneDay[1] > this.formSquat);
      return filtered.pop()[0] - 1;
    },
    resBench() {
      // const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}raw`].Bench;
      const res = [[0, 1]];
      const filtered = res.filter((oneDay) => oneDay[1] > this.formBench);
      return filtered.pop()[0] - 1;
    },
    resDeadlift() {
      // const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}raw`].Deadlift;
      const res = [[0, 1]];
      const filtered = res.filter((oneDay) => oneDay[1] > this.formDeadlift);
      return filtered.pop()[0] - 1;
    },
    resTotal() {
      // const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}raw`].Total;
      const res = [[0, 1]];
      const filtered = res.filter((oneDay) => oneDay[1] > this.formTotal);
      return filtered.pop()[0] - 1;
    },
  },
  methods: {
    resetOption() {
      // this.weightClass = 'N/A';
      console.log(this.weightClass);
      console.log('HELLO');
      if (!(this.weightClassSelections.includes(this.weightClass))) {
        this.showData = false;
      } else {
        this.showData = true;
      }
      // this.weightClass = '';
    },
    showClassData() {
      console.log('Test');
      if (this.weightClass === '') {
        this.classData2 = [];
      }
      const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}raw`;
      console.log(wClassCode);
      this.classData2 = allData[wClassCode];
    },
  },
};
</script>
