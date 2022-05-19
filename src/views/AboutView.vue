<template>
  <div class="about">
    <h2>Select a weight class, enter your lifts, see how you rank</h2>
      <div id="app">
            <div>
    <v-container fluid>
    <v-row>
      <v-col>
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
              :items="ageSelections"
              v-model="age"
              item-text="text"
              item-value="value"
              label="Age Bracket"
              :menu-props="{maxHeight: 804}"
              :outlined=true
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
      <v-col>
              <v-switch
               v-model="unit"
               true-value=lb
               false-value=kg
              :label="`Units: ${unit.toString()}`"
               ></v-switch>
              </v-col>
      <!-- <v-spacer>
      </v-spacer> -->
      </v-row>

    <v-row>
      <v-col justify="center">
            <v-text-field
            v-model="formSquat"
            :outlined=true
            type="number"
            :label="`Enter Squat 1RM (${unit})`"
          ></v-text-field>
            </v-col>
            <v-col justify="center">
                <v-switch
               v-if="testedStatus==='Untested'"
               v-model="knee"
               true-value='Wraps'
               false-value='Raw'
              :label="`${kneeLabel.toString()}`"
               ></v-switch>
            </v-col>
    </v-row>

    <v-row>

            <v-col  justify="center">
            <v-text-field
            v-model="formBench"
            :outlined=true
            type="number"
            :label="`Enter Bench 1RM (${unit})`"
          ></v-text-field>
            <v-text-field
            v-model="formDeadlift"
            :outlined=true
            type="number"
            :label="`Enter Deadlift 1RM (${unit})`"
          ></v-text-field>

            <v-text-field
            v-model="formTotal"
            :outlined=true
            type="number"
            :label="`Enter Best Total (${unit})`"
          ></v-text-field>
            </v-col>
          <v-col justify="center">
            <v-btn
            color="white"
            class="mr-4"
            @click="resetLifts()"
          >
            Reset Lifts
          </v-btn>
           </v-col>
            </v-row>

</v-container>
            </div>

   <v-container fluid>
          <v-row v-if="showData">
      <v-col justify="start">
        <v-row no-gutters> <h3>Compared to all lifters in the {{gender}} {{weightClass}}kg
        ({{returnAgeClass(age)}}) ({{testedStatus}} {{knee}})
        class - approx {{classDataSize}} lifters:</h3>
        </v-row>
        <v-row no-gutters>
      Your Squat is better or equal than {{resSquat}}%
      </v-row >
        <v-row no-gutters>
      Your Bench is better or equal than {{resBench}}%
      </v-row>
        <v-row no-gutters>
      Your Deadlift is better or equal than {{resDeadlift}}%
      </v-row>
        <v-row no-gutters>
      Your Total is better or equal than {{resTotal}}%
      </v-row>
      </v-col>
          </v-row>
 </v-container>

 <v-container fluid>
     <v-row v-if="showData" no-gutters>
                 <v-switch
               v-model="percentilesShown"
               true-value=1
               false-value=5
              :label="'Show Detailed Percentile Data'"
               ></v-switch>
               </v-row>
<v-row v-if="showData">
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
</v-row>
<v-row v-if="!showData" no-gutters>
  <h3  style="color:red">Please Select a Weight Class</h3>
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
            { text: '116 lbs (Under 116)', value: '53' },
            { text: '130 lbs (Between 116.0 -> 130.0)', value: '59' },
            { text: '145 lbs (Between 130.0 -> 145.0)', value: '66' },
            { text: '163 lbs (Between 145.0 -> 163.0)', value: '74' },
            { text: '183 lbs (Between 163.0 -> 185.0)', value: '83' },
            { text: '205 lbs (Between 183.0 -> 205.0)', value: '93' },
            { text: '231 lbs (Between 205.0 -> 231.0)', value: '105' },
            { text: '264 lbs (Between 231.0 -> 264.0)', value: '120' },
            { text: '264+ lbs (Over 264)', value: '120+' },
          ],
          MaleUntested: [
            { text: '114 lbs (Under 114)', value: '52' },
            { text: '123 lbs (Between 114.0 -> 123.0)', value: '56' },
            { text: '132 lbs (Between 123.0 -> 132.0)', value: '60' },
            { text: '148 lbs (Between 132.0 -> 148.0)', value: '67.5' },
            { text: '165 lbs (Between 148.0 -> 165.0)', value: '75' },
            { text: '181 lbs (Between 165.0 -> 181.0)', value: '82.5' },
            { text: '198 lbs (Between 181.0 -> 198.0)', value: '90' },
            { text: '220 lbs (Between 198.0 -> 220.0)', value: '100' },
            { text: '242 lbs (Between 220.0 -> 242.0)', value: '110' },
            { text: '275 lbs (Between 242.0 -> 275.0)', value: '125' },
            { text: '308 lbs (Between 275.0 -> 308.0)', value: '140' },
            { text: '308+ lbs (Over 308)', value: '140+' },
          ],
          FemaleTested: [
            { text: '94 lbs (Under 94)', value: '43' },
            { text: '103 lbs (Between 95.0 -> 103.0)', value: '47' },
            { text: '114 lbs (Between 103.0 -> 114.0)', value: '52' },
            { text: '125 lbs (Between 114.0 -> 125.0)', value: '57' },
            { text: '138 lbs (Between 125.0 -> 138.0)', value: '63' },
            { text: '152 lbs (Between 138.0 -> 152.0)', value: '69' },
            { text: '168 lbs (Between 152.0 -> 168.0)', value: '76' },
            { text: '185 lbs (Between 168.0 -> 185.0)', value: '84' },
            { text: '185+ lbs (Over 185)', value: '84+' },
          ],
          FemaleUntested: [
            { text: '97 lbs (Under 97)', value: '44' },
            { text: '105 lbs (Between 97.0 -> 105.0)', value: '48' },
            { text: '114 lbs (Between 105.0 -> 114.0)', value: '52' },
            { text: '123 lbs (Between 114.0 -> 123.0)', value: '56' },
            { text: '132 lbs (Between 123.0 -> 132.0)', value: '60' },
            { text: '148 lbs (Between 132.0 -> 148.0)', value: '67' },
            { text: '165 lbs (Between 148.0 -> 165.0)', value: '75' },
            { text: '181 lbs (Between 165.0 -> 181.0)', value: '82' },
            { text: '198 lbs (Between 181.0 -> 198.0)', value: '90' },
            { text: '198+ lbs (Over 198)', value: '90+' },
          ],
        },
      },
      weightClass: '',
      testedSelections: [
        { text: 'Drug Tested Only', value: 'Tested' },
        { text: 'All Results', value: 'Untested' },
      ],
      testedStatus: 'Tested',
      ageSelections: [
        { text: 'All Ages', value: 'AA' },
        { text: '13 to 15 years', value: 'T1' },
        { text: '16 to 19 years', value: 'T2' },
        { text: '20 to 23 years', value: 'J1' },
        { text: '24 to 39 years', value: 'O1' },
        { text: '40 to 59 years', value: 'M1' },
        { text: '60+ years', value: 'M2' },
      ],
      formSquat: '',
      formBench: '',
      formDeadlift: '',
      formTotal: '',
      showData: false,
      percentilesShown: 5,
      unit: 'kg',
      knee: 'Raw',
      age: 'AA',
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

      const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}${this.knee}${this.age}`;
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
      const wClassCode = `${this.gender.slice(0, 1)}${this.weightClass}${this.knee}${this.age}`;
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
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}${this.knee}${this.age}`].Squat;
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
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}${this.knee}${this.age}`].Bench;
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

      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}${this.knee}${this.age}`].Deadlift;
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
      const res = allData[`${this.gender.slice(0, 1)}${this.weightClass}${this.knee}${this.age}`].Total;
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
    },
    showTableData() {
      this.showData = true;
    },
    returnAgeClass(ageCode) {
      const age = this.ageSelections.filter((arr) => arr.value === ageCode);
      return age[0].text;
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
