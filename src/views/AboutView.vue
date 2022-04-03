<template>
  <div class="about">
    <h2>Select a weight class, enter your lifts, see how you rank</h2>
      <div id="app">
            <div>
              <v-container fluid>
    <v-row>
      <!-- <v-col cols="4" justify="center">
      </v-col> -->
    <v-col cols="12" justify="center">
        <v-select
              :items="genderSelections"
              v-model="gender"
              item-text="name"
              item-value="id"
              label="Gender"
              :outlined=true
              @change="resetOption()"
            ></v-select>

            <v-select
              :items="testedSelections"
              v-model="testedStatus"
              item-text="text"
              item-value="value"
              label="Division"
              :outlined=true
              @change="resetOption()"
            ></v-select>

            <v-row>

              <v-col cols="8" justify="center">
              <v-select
                :items="weightClassSelections"
                v-model="weightClass"
                item-text="text"
                item-value="value"
                :outlined=true
                :menu-props="{maxHeight: 804}"
                :label="`Weight Class (${unit})`"
                @change="showTableData()"
              ></v-select>
              </v-col>

            <v-col cols="4" justify="center">
              <v-switch
               v-model="unit"
               true-value=lb
               false-value=kg
              :label="`Units: ${unit.toString()}`"
               ></v-switch>
              </v-col>

            </v-row>
            </v-col>
<v-container fluid>
    <v-row>
            <v-col cols="8" justify="center">
            <v-text-field
            v-model="formSquat"
            :outlined=true
            type="number"
            :label="`Enter your Squat 1RM (${unit})`"
          ></v-text-field>
            </v-col>
            <v-col cols="4" justify="center">
                <v-switch
               v-if="testedStatus==='Untested'"
               v-model="knee"
               true-value='Wraps'
               false-value='Raw'
              :label="`${kneeLabel.toString()}`"
               ></v-switch>
            </v-col>
    </v-row>
</v-container>

<v-container fluid>
    <v-row>
            <v-col cols="8" justify="center">
            <v-text-field
            v-model="formBench"
            :outlined=true
            type="number"
            :label="`Enter your Bench 1RM (${unit})`"
          ></v-text-field>
            <v-text-field
            v-model="formDeadlift"
            :outlined=true
            type="number"
            :label="`Enter your Deadlift 1RM (${unit})`"
          ></v-text-field>
           </v-col>
    </v-row>

</v-container>
          <v-row>
            <v-col cols="8" justify="center">

            <v-text-field
            v-model="formTotal"
            :outlined=true
            type="number"
            :label="`Enter your best Total (${unit})`"
          ></v-text-field>
            </v-col>
          <v-col cols="3" justify="center">
            <v-btn
            color="white"
            class="mr-3"
            @click="resetLifts()"
          >
            Reset
          </v-btn>
           </v-col>
            </v-row>

            </v-row>
            </v-container>
            </div>

            <v-container fluid>
          <v-row v-if="showData">
      <v-col cols="12" justify="center">
        <v-row> <h3>Compared to all lifters in the {{gender}} {{weightClass}}
          ({{testedStatus}} {{knee}}) class - approx {{classDataSize}} lifters:</h3>
        </v-row>
        <v-row>
      Your Squat is better or equal than {{resSquat}}%
      </v-row>
        <v-row>
      Your Bench is better or equal than {{resBench}}%
      </v-row>
        <v-row>
      Your Deadlift is better or equal than {{resDeadlift}}%
      </v-row>
        <v-row>
      Your Total is better or equal than {{resTotal}}%
      </v-row>
      </v-col>
          </v-row>
 </v-container>
 <v-container fluid>
     <v-row v-if="showData">
                 <v-switch
               v-model="percentilesShown"
               true-value=1
               false-value=5
              :label="'Show Detailed Percentile Data'"
               ></v-switch>
               </v-row>
<v-row v-if="showData">
<!-- <v-col cols="3" justify="center">
      </v-col> -->
  <v-col cols="3" justify="center">
    <tr><h4>Pecentile</h4></tr>
     <tr v-for="(item) of classData.Squat" :key="item">{{item[0]}}</tr>
  </v-col>
      <v-col cols="2" justify="center">
       <tr><h4>Squat</h4></tr>
        <tr v-for="item of classData.Squat"
        :key="item">{{myRound(item[1] * unitMultiplier)}}</tr>
      </v-col>
      <v-col cols="2" justify="center">
        <tr><h4>Bench</h4></tr>
        <tr v-for="item of classData.Bench"
        :key="item"> {{myRound(item[1] * unitMultiplier)}}</tr>
      </v-col>
      <v-col cols="2" justify="center">
        <tr><h4>Deadlift</h4></tr>
        <tr v-for="item of classData.Deadlift"
        :key="item"> {{myRound(item[1] * unitMultiplier)}}</tr>
      </v-col>
      <v-col cols="2" justify="center">
        <tr><h4>Total</h4></tr>
        <tr v-for="item of classData.Total"
        :key="item"> {{myRound(item[1] * unitMultiplier)}}</tr>
      </v-col>
      * Note Bench and Deadlift tables include single lift competition results
        <!-- <ul><li v-for="elem in row" :key="elem">{{elem}}</li></ul> -->
</v-row>
<v-row v-if="!showData">
  <h3>Please Select a Weight Class</h3>
</v-row>
 </v-container>
  </div>

  </div>
</template>

<script>
import allData from '../../all_data.json';

export default {
  data() {
    return {
      genderSelections: ['Male', 'Female'],
      gender: 'Male',
      weightClassAll: {
        kg: {
          MaleTested: [
            { text: '53 kg (Under 53.0)', value: '53' },
            { text: '59 kg (Between 53.0 -> 59.0)', value: '59' },
            { text: '66 kg (Between 59.0 -> 66.0)', value: '66' },
            { text: '74 kg (Between 66.0 -> 74.0)', value: '74' },
            { text: '83 kg (Between 74.0 -> 83.0)', value: '83' },
            { text: '93 kg (Between 83.0 -> 93.0)', value: '93' },
            { text: '105 kg (Between 93.0 -> 105.0)', value: '105' },
            { text: '120 kg (Between 105.0 -> 120.0)', value: '120' },
            { text: '120+ kg (Over 120)', value: '120+' },
          ],
          MaleUntested: [
            { text: '52 kg (Under 52)', value: '52' },
            { text: '56 kg (Between 52.0 -> 56.0)', value: '56' },
            { text: '60 kg (Between 56.0 -> 60.0)', value: '60' },
            { text: '67.5 kg (Between 60.0 -> 67.5)', value: '67.5' },
            { text: '75 kg (Between 67.5 -> 75.0)', value: '75' },
            { text: '82.5 kg (Between 75.0 -> 82.5)', value: '82.5' },
            { text: '90 kg (Between 82.5 -> 90.0)', value: '90' },
            { text: '100 kg (Between 90.0 -> 100.0)', value: '100' },
            { text: '110 kg (Between 100.0 -> 110.0)', value: '110' },
            { text: '125 kg (Between 110.0 -> 125.0)', value: '125' },
            { text: '140 kg (Between 125.0 -> 140.0)', value: '140' },
            { text: '140+ kg (Over 140)', value: '140+' },
          ],
          FemaleTested: [
            { text: '43 kg (Under 43)', value: '43' },
            { text: '47 kg (Between 43.0 -> 47.0)', value: '47' },
            { text: '52 kg (Between 47.0 -> 52.0)', value: '52' },
            { text: '57 kg (Between 52.0 -> 57.0)', value: '57' },
            { text: '63 kg (Between 57.0 -> 63.0)', value: '63' },
            { text: '69 kg (Between 63.0 -> 69.0)', value: '69' },
            { text: '76 kg (Between 69.0 -> 76.0)', value: '76' },
            { text: '84 kg (Between 76.0 -> 84.0)', value: '84' },
            { text: '84+ kg (Over 84)', value: '84+' },
          ],
          FemaleUntested: [
            { text: '44 kg (Under 44)', value: '44' },
            { text: '48 kg (Between 44.0 -> 48.0)', value: '48' },
            { text: '52 kg (Between 48.0 -> 52.0)', value: '52' },
            { text: '56 kg (Between 52.0 -> 56.0)', value: '56' },
            { text: '60 kg (Between 56.0 -> 60.0)', value: '60' },
            { text: '67.5 kg (Between 60.0 -> 67.5)', value: '67.5' },
            { text: '75 kg (Between 67.5 -> 75.0)', value: '75' },
            { text: '82.5 kg (Between 75.0 -> 82.5)', value: '82.5' },
            { text: '90 kg (Between 82.5 -> 90.0)', value: '90' },
            { text: '90+ kg (Over 90)', value: '90+' },
          ],
        },
        lb: {
          MaleTested: [
            { text: '116', value: '53' },
            { text: '130', value: '59' },
            { text: '145', value: '66' },
            { text: '163', value: '74' },
            { text: '183', value: '83' },
            { text: '205', value: '93' },
            { text: '231', value: '105' },
            { text: '264', value: '105' },
            { text: '264+', value: '120+' },
          ],
          MaleUntested: [
            { text: '114', value: '52' },
            { text: '123', value: '56' },
            { text: '132', value: '60' },
            { text: '148', value: '67.5' },
            { text: '165', value: '75' },
            { text: '181', value: '82.5' },
            { text: '198', value: '90' },
            { text: '220', value: '100' },
            { text: '242', value: '110' },
            { text: '275', value: '125' },
            { text: '308', value: '140' },
            { text: '308+', value: '140+' },
          ],
          FemaleTested: [
            { text: '94', value: '43' },
            { text: '103', value: '47' },
            { text: '114', value: '52' },
            { text: '125', value: '57' },
            { text: '138', value: '63' },
            { text: '152', value: '69' },
            { text: '168', value: '76' },
            { text: '185', value: '84' },
            { text: '185+', value: '84' },
          ],
          FemaleUntested: [
            { text: '97', value: '44' },
            { text: '105', value: '48' },
            { text: '114', value: '52' },
            { text: '123', value: '56' },
            { text: '132', value: '60' },
            { text: '148', value: '67' },
            { text: '165', value: '75' },
            { text: '181', value: '82' },
            { text: '198', value: '90' },
            { text: '198+', value: '90' },
          ],
        },
      },
      weightClass: '',
      testedSelections: [
        { text: 'Drug Tested Only', value: 'Tested' },
        { text: 'All Results', value: 'Untested' },
      ],
      testedStatus: 'Tested',
      formSquat: '',
      formBench: '',
      formDeadlift: '',
      formTotal: '',
      showData: false,
      percentilesShown: 5,
      unit: 'kg',
      knee: 'Raw',

    };
  },
  computed: {
    derivedWeightClass() {
      if (this.weightClass === '') {
        return '';
      }
      if (this.unit === 'lb') {
        const index = this.weightClassAll.lb[`${this.gender}${this.testedStatus}`].indexOf(this.weightClass);
        return this.weightClassAll.kg[`${this.gender}${this.testedStatus}`][index];
      }

      return this.weightClass;
    },
    unitMultiplier() {
      return (this.unit === 'kg' ? 1 : 2.204);
    },
    kneeLabel() {
      return (this.knee === 'Raw' ? 'Sleeves' : 'Wraps');
    },
    classData() {
      const reducedWeightClasses = this.weightClassSelections.map((item) => item.value);
      if (reducedWeightClasses.includes(this.weightClass) === false) {
        return [];
      }

      const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}${this.knee}`;
      const fullData = allData[wClassCode];
      const minData = {};
      const keys = Object.keys(fullData);

      keys.forEach((key) => {
        minData[key] = fullData[key].filter((item) => item[0] % this.percentilesShown === 0);
      });
      return minData;
    },
    classDataSize() {
      const reducedWeightClasses = this.weightClassSelections.map((item) => item.value);
      if (reducedWeightClasses.includes(this.weightClass) === false) {
        return '';
      }
      const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}${this.knee}`;
      const classSize = allData[wClassCode].Total[1][2];
      return classSize * 100;
    },
    weightClassSelections() {
      const wclass = `${this.gender}${this.testedStatus}`;
      return this.weightClassAll[this.unit][wclass];
    },
    resSquat() {
      if (this.weightClass === '') {
        return 0;
      }
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}${this.knee}`].Squat;
      const filtered = res.filter((oneDay) => oneDay[1] > this.formSquat / this.unitMultiplier);
      if (filtered.length === 0) {
        return 100;
      }
      return filtered.pop()[0] - 1;
    },
    resBench() {
      if (this.weightClass === '') {
        return 0;
      }
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}${this.knee}`].Bench;
      const filtered = res.filter((oneDay) => oneDay[1] > this.formBench / this.unitMultiplier);
      if (filtered.length === 0) {
        return 100;
      }
      return filtered.pop()[0] - 1;
    },
    resDeadlift() {
      if (this.weightClass === '') {
        return 0;
      }

      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}${this.knee}`].Deadlift;
      const filtered = res.filter((oneDay) => oneDay[1] > this.formDeadlift / this.unitMultiplier);
      if (filtered.length === 0) {
        return 100;
      }
      return filtered.pop()[0] - 1;
    },
    resTotal() {
      if (this.weightClass === '') {
        return 0;
      }
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}${this.knee}`].Total;
      const filtered = res.filter((oneDay) => oneDay[1] > this.formTotal / this.unitMultiplier);
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
      this.knee = 'Raw';
      // this.weightClass = '';
    },
    showTableData() {
      this.showData = true;
    },
    myRound(num) {
      if (Number.isInteger(num)) {
        return num;
      }
      return num.toFixed(1);
    },
    resetLifts() {
      this.formSquat = '';
      this.formBench = '';
      this.formDeadlift = '';
      this.formTotal = '';
    },

  },
};
</script>
