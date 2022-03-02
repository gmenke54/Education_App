<template>
  <div v-if="this.students">
    <div>Our Students</div>
    <div>
      <div class="nut">{{this.title.display}}</div>
      <DoughnutChart :chartData="edData" :options="this.options" />
    </div>
    <div class="btn-bar">
      <div class="btn" @click="this.title={display: 'Middle Initial', data: 'initial'}">Middle Initial</div>
      <div class="btn" @click="this.title={display: 'Last Name', data: 'l_name'}">Last Name</div>
      <div class="btn" @click="this.title={display: 'Prefix', data: 'prefix'}">Prefix</div>
      <div class="btn" @click="this.title={display: 'State', data: 'state'}">State</div>
      <div class="btn" @click="this.title={display: 'Zip Code', data: 'zip'}">Zip Code</div>
      <div class="btn" @click="this.title={display: 'Education Level', data: 'education'}">Education Level</div>
      <div class="btn" @click="this.title={display: 'Income Level', data: 'income'}">Income Level</div>
    </div>
  </div>
</template>

<script>
import { DoughnutChart } from 'vue-chart-3';
import axios from 'axios'
export default {
  name: 'App',
  components: {
    DoughnutChart,
  },
  mounted(){
    this.getStudents()
  },
  data() {
    return {
      title: {display: 'Education', data: 'education'},
      options: {
        plugins: {
          legend: {
            display: false
          }
        }
      },
      students: null
    }
  },
  methods: {
    async getStudents(){
      const res = await axios.get('http://localhost:3000/students/')
      this.students = res.data
    }
  },
  computed: {
    edData(){
      let edMap = {}
      let curField = this.title.data
      for (let i=0; i<this.students.length; i++){
        if (!(this.students[i][curField] in edMap)){
          edMap[this.students[i][curField]] = 1 
        } else {
          edMap[this.students[i][curField]]++
        }
      }
      let labelsArr = [] 
      let dataArr = []
      let colorArr = []
      for (const property in edMap) {
        const string = property.toLowerCase()
        const arr = string.split(" ")
        for (let i=0; i<arr.length; i++){
          arr[i] = arr[i].charAt(0).toUpperCase() + arr[i].slice(1);
        }
        const finalStr = arr.join(" ")
        labelsArr.push(finalStr)
        dataArr.push((100 * edMap[property]/this.students.length).toFixed(1))
        colorArr.push(`#${Math.floor(Math.random()*16777215).toString(16)}`)
      }
      return {
        labels: labelsArr,
        datasets: [
          {
            data: dataArr,
            backgroundColor: colorArr,
          },
        ],
      }
    },
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
