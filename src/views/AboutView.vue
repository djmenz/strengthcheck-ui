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
              @change="resetOption()"
            ></v-select>

            <v-select
              :items="testedSelections"
              v-model="testedStatus"
              item-text="name"
              item-value="id"
              label="tested/untested"
              @change="resetOption()"
            ></v-select>

            <v-select
              :items="weightClassSelections"
              v-model="weightClass"
              item-text="name"
              item-value="id"
              :label="`Weight Class (${unit})`"
              @change="showTableData()"
            ></v-select>
                <v-switch
               v-model="detailSwitch"
              :label="`Detailed Data: ${detailSwitch.toString()}`"
               ></v-switch>

            </v-col>
            <v-col cols="2" justify="center">
            <v-text-field
            v-model="formSquat"
            :label="`Squat 1RM (${unit})`"
          ></v-text-field>
            <v-text-field
            v-model="formBench"
            :label="`Bench 1RM (${unit})`"
          ></v-text-field>
            <v-text-field
            v-model="formDeadlift"
            :label="`Deadlift 1RM (${unit})`"
          ></v-text-field>
            <v-text-field
            v-model="formTotal"
            :label="`Total (${unit})`"
          ></v-text-field>
            </v-col>
            </v-row>
            </v-container>
            </div>
          <v-row v-if="showData">
            <v-col cols="3" justify="center">
      </v-col>
      <v-col cols="6" justify="center">
        <v-row> <b>Compared to the lifters in the {{gender}} {{weightClass}}
          ({{testedStatus}}) class - approx {{classDataSize}} lifters:</b>
        </v-row>
        <v-row>
      Your Squat is better or equal to {{resSquat}}%
      </v-row>
        <v-row>
      Your Bench is better than {{resBench}}%
      </v-row>
        <v-row>
      Your Deadlift is better than {{resDeadlift}}%
      </v-row>
        <v-row>
      Your Total is better than {{resTotal}}%
      </v-row>
      </v-col>

          </v-row>
<v-row v-if="showData">
<v-col cols="3" justify="center">
      </v-col>
  <v-col cols="1" justify="center">
    <tr><h2>Pecentile</h2></tr>
     <tr v-for="(item) of classData.Squat" :key="item">{{item[0]}}</tr>
  </v-col>
      <v-col cols="1" justify="center">
       <tr><h2>Squat</h2></tr>
        <tr v-for="item of classData.Squat" :key="item">{{item[1]}}</tr>
      </v-col>
      <v-col cols="1" justify="center">
        <tr><h2>Bench</h2></tr>
        <tr v-for="item of classData.Bench" :key="item"> {{item[1]}}</tr>
      </v-col>
      <v-col cols="1" justify="center">
        <tr><h2>Deadlift</h2></tr>
        <tr v-for="item of classData.Deadlift" :key="item"> {{item[1]}}</tr>
      </v-col>
      <v-col cols="1" justify="center">
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
      showData: false,
      detailSwitch: false,
      unit: 'kg',

    };
  },
  computed: {
    classData() {
      if (this.weightClassSelections.includes(this.weightClass) === false) {
        return [];
      }
      const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}raw`;
      console.log(wClassCode);
      const fullData = allData[wClassCode];
      const minData = {};
      const keys = Object.keys(fullData);

      const percentileScale = this.detailSwitch ? 1 : 5;

      keys.forEach((key, index) => {
        console.log(`${key} ${index}`);
        console.log(fullData[key]);
        minData[key] = fullData[key].filter((item) => item[0] % percentileScale === 0);
      });
      return minData;
    },
    classDataSize() {
      if (this.weightClassSelections.includes(this.weightClass) === false) {
        return '';
      }
      const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}raw`;
      console.log(wClassCode);
      const classSize = allData[wClassCode].Total[1][3];
      return classSize * 100;
    },
    weightClassSelections() {
      const wclass = `${this.gender}${this.testedStatus}`;
      console.log(wclass);
      return this.weightClassAll[wclass];
    },
    resSquat() {
      if (this.weightClass === '') {
        return 0;
      }
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}raw`].Squat;
      // const res = [[0, 1]];
      const filtered = res.filter((oneDay) => oneDay[1] > this.formSquat);
      if (filtered.length === 0) {
        return 100;
      }
      return filtered.pop()[0] - 1;
    },
    resBench() {
      if (this.weightClass === '') {
        return 0;
      }
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}raw`].Bench;
      const filtered = res.filter((oneDay) => oneDay[1] > this.formBench);
      if (filtered.length === 0) {
        return 100;
      }
      return filtered.pop()[0] - 1;
    },
    resDeadlift() {
      if (this.weightClass === '') {
        console.log('breaks');
        return 0;
      }
      console.log('check');
      console.log(this.allData);

      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}raw`].Deadlift;
      // const res = [[0, 1]];
      const filtered = res.filter((oneDay) => oneDay[1] > this.formDeadlift);
      if (filtered.length === 0) {
        return 100;
      }
      return filtered.pop()[0] - 1;
    },
    resTotal() {
      if (this.weightClass === '') {
        return 0;
      }
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}raw`].Total;
      // const res = [[0, 1]];
      const filtered = res.filter((oneDay) => oneDay[1] > this.formTotal);
      if (filtered.length === 0) {
        return 100;
      }
      return filtered.pop()[0] - 1;
    },
  },
  methods: {
    resetOption() {
      this.weightClass = 'N/A';
      this.showData = false;
      // this.weightClass = '';
    },
    showTableData() {
      this.showData = true;
      console.log('show data?');
    },
    // showClassData() {
    //   console.log('Test');
    //   if (this.weightClass === '') {
    //     this.classData2 = [];
    //   }
    //   const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}raw`;
    //   console.log(wClassCode);
    //   this.classData2 = allData[wClassCode];
    // },
    // genData(){
    //   if (this.weightClassSelections.includes(this.weightClass) === false) {
    //     return [];
    //   }
    //   const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}raw`;
    //   console.log(wClassCode);
    //   return allData[wClassCode];
    // },
  },
};
</script>
