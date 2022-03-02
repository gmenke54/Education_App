<template>
  <div v-if="this.students">
    <div>{{this.title}}</div>
    <DoughnutChart :chartData="edData" :options="this.options" />
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
      title: 'Education',
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
      for (let i=0; i<this.students.length; i++){
        console.log(this.students[i])
        if (!(this.students[i].education in edMap)){
          console.log('not found')
          edMap[this.students[i].education] = 1 
        } else {
          edMap[this.students[i].education]++
        }
      }
      console.log(edMap)
      let labelsArr = [] 
      let dataArr = []
      let colorArr = []
      for (const property in edMap) {
        labelsArr.push(property)
        dataArr.push(edMap[property])
        colorArr.push(`#${Math.floor(Math.random()*16777215).toString(16)}`)
      }
      console.log(labelsArr, colorArr, dataArr)
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
