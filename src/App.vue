<template>
  <div v-if="this.students">
    <div class="title">Students</div>
    <div class="flex-row">
      <div class="btn-bar">
        <div class="btn" @click="this.title={display: 'Education Level', data: 'education'}, setActiv('education')" :class="education">Education Level</div>
        <div class="btn" @click="this.title={display: 'Income Level', data: 'income'}, setActiv('income')" :class="income">Income Level</div>
        <div class="btn" @click="this.title={display: 'State', data: 'state'}, setActiv('state')" :class="state">State</div>
        <div class="btn" @click="this.title={display: 'Prefix', data: 'prefix'}, setActiv('prefix')" :class="prefix">Prefix</div>
        <div class="btn" @click="this.title={display: 'Last Name', data: 'l_name'}, setActiv('l_name')" :class="l_name">Last Name</div>
        <div class="btn" @click="this.title={display: 'Middle Initial', data: 'initial'}, setActiv('initial')" :class="initial">Middle Initial</div>
      </div>
      <div class="nut">
        <div class="hole">{{this.title.display}}</div>
        <DoughnutChart :chartData="edData" :options="this.options" />
      </div>
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
      students: null,
      activ: 'education',
      initial: 'grey',
      l_name: 'grey',
      prefix: 'grey',
      state: 'grey',
      income: 'grey',
      education: 'black',
    }
  },
  methods: {
    async getStudents(){
      const res = await axios.get('http://localhost:3000/students/')
      this.students = res.data
    },
    setActiv(level){
      this.activ = level
      if (level === 'initial'){
        this.initial='black'
        this.l_name='grey'
        this.prefix='grey'
        this.state='grey'
        this.income='grey'
        this.education='grey'
      } else if (level === 'l_name'){
        this.initial='grey'
        this.l_name='black'
        this.prefix='grey'
        this.state='grey'
        this.income='grey'
        this.education='grey'
      } else if (level === 'prefix'){
        this.initial='grey'
        this.l_name='grey'
        this.prefix='black'
        this.state='grey'
        this.income='grey'
        this.education='grey'
      } else if (level === 'state'){
        this.initial='grey'
        this.l_name='grey'
        this.prefix='grey'
        this.state='black'
        this.income='grey'
        this.education='grey'
      } else if (level === 'income'){
        this.initial='grey'
        this.l_name='grey'
        this.prefix='grey'
        this.state='grey'
        this.income='black'
        this.education='grey'
      } else {
        this.initial='grey'
        this.l_name='grey'
        this.prefix='grey'
        this.state='grey'
        this.income='grey'
        this.education='black'
      }
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
  color: #4F5451;
  font-weight: 700;
  font-size: 25px
}
.nut{
  position: relative;
  display: flex;
  justify-content: center;
  margin: 10px;
}
.hole{
  position: absolute;
  top: 46.5%;
  text-align: center;
}
.btn{
  cursor: pointer;
}
.btn:hover{
  color: black
}
.btn-bar{
  width: 225px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-left: 3px solid black;
  padding-left: 5px;
  padding-top: 7px;
  padding-bottom: 6px;
  margin-bottom: 15px;
}
.flex-row{
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap
}
.title{
  margin: 15px;
  color: black;
  font-size: 30px;
}
.black{
  color: black
}
</style>
