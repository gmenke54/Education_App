<template>
  <div class="cont" v-if="this.students">
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
      const colorOptions = [
        '#F44336',
        '#FFEBEE',
        '#FFCDD2',
        '#EF9A9A',
        '#E57373',
        '#EF5350',
        '#E53935',
        '#D32F2F',
        '#C62828',
        '#B71C1C',
        '#FF8A80',
        '#FF5252',
        '#FF1744',
        '#D50000',
        '#FCE4EC',
        '#F8BBD0',
        '#F48FB1',
        '#F06292',
        '#EC407A',
        '#E91E63',
        '#D81B60',
        '#C2185B',
        '#AD1457',
        '#880E4F',
        '#FF80AB',
        '#FF4081',
        '#F50057',
        '#C51162',
        '#F3E5F5',
        '#E1BEE7',
        '#CE93D8',
        '#BA68C8',
        '#AB47BC',
        '#9C27B0',
        '#8E24AA',
        '#7B1FA2',
        '#6A1B9A',
        '#4A148C',
        '#EA80FC',
        '#E040FB',
        '#D500F9',
        '#AA00FF',
        '#EDE7F6',
        '#D1C4E9',
        '#B39DDB',
        '#9575CD',
        '#7E57C2',
        '#673AB7',
        '#5E35B1',
        '#512DA8',
        '#4527A0',
        '#311B92',
        '#B388FF',
        '#7C4DFF',
        '#651FFF',
        '#6200EA',
        '#E8EAF6',
        '#C5CAE9',
        '#9FA8DA',
        '#7986CB',
        '#5C6BC0',
        '#3F51B5',
        '#3949AB',
        '#303F9F',
        '#283593',
        '#1A237E',
        '#8C9EFF',
        '#536DFE',
        '#3D5AFE',
        '#304FFE',
        '#E3F2FD',
        '#BBDEFB',
        '#90CAF9',
        '#64B5F6',
        '#42A5F5',
        '#2196F3',
        '#1E88E5',
        '#1976D2',
        '#1565C0',
        '#0D47A1',
        '#82B1FF',
        '#448AFF',
        '#2979FF',
        '#2962FF',
        '#E1F5FE',
        '#B3E5FC',
        '#81D4FA',
        '#4FC3F7',
        '#29B6F6',
        '#03A9F4',
        '#039BE5',
        '#0288D1',
        '#0277BD',
        '#01579B',
        '#80D8FF',
        '#40C4FF',
        '#00B0FF',
        '#0091EA',
        '#E0F7FA',
        '#B2EBF2',
        '#80DEEA',
        '#4DD0E1',
        '#26C6DA',
        '#00BCD4',
        '#00ACC1',
        '#0097A7',
        '#00838F',
        '#006064',
        '#84FFFF',
        '#18FFFF',
        '#00E5FF',
        '#00B8D4',
        '#E0F2F1',
        '#B2DFDB',
        '#80CBC4',
        '#4DB6AC',
        '#26A69A',
        '#009688',
        '#00897B',
        '#00796B',
        '#00695C',
        '#004D40',
        '#A7FFEB',
        '#64FFDA',
        '#1DE9B6',
        '#00BFA5',
        '#E8F5E9',
        '#C8E6C9',
        '#A5D6A7',
        '#81C784',
        '#66BB6A',
        '#4CAF50',
        '#43A047',
        '#388E3C',
        '#2E7D32',
        '#1B5E20',
        '#B9F6CA',
        '#69F0AE',
        '#00E676',
        '#00C853',
        '#F1F8E9',
        '#DCEDC8',
        '#C5E1A5',
        '#AED581',
        '#9CCC65',
        '#8BC34A',
        '#7CB342',
        '#689F38',
        '#558B2F',
        '#33691E',
        '#CCFF90',
        '#B2FF59',
        '#76FF03',
        '#64DD17',
        '#F9FBE7',
        '#F0F4C3'
      ]
      for (const property in edMap) {
        const string = property.toLowerCase()
        const arr = string.split(" ")
        for (let i=0; i<arr.length; i++){
          arr[i] = arr[i].charAt(0).toUpperCase() + arr[i].slice(1);
        }
        const finalStr = arr.join(" ")
        labelsArr.push(`${finalStr} (%)`)
        dataArr.push((100 * edMap[property]/this.students.length).toFixed(1))
        // Random Color from curated collection:
        colorArr.push(colorOptions[Math.floor(Math.random()*150)])
        // Random color from all possible hex codes:
        // colorArr.push(`#${Math.floor(Math.random()*16777215).toString(16)}`)
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
  font-size: 25px;
  background-color: #F0F2F4;  
  min-height: 100vh;

}
body{
  margin: 0;
}
.nut{
  position: relative;
  display: flex;
  justify-content: center;
  margin: 10px;
  box-shadow: 0px 0px 12px -5px rgba(0,0,0,0.7);
  border-radius: 50%;
  padding: 5px;
  background: white;
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
  width: 200px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-left: 3px solid black;
  padding-left: 5px;
  padding-top: 7px;
  padding-bottom: 6px;
  margin-bottom: 15px;
  box-shadow: 0px 0px 12px -5px rgba(0,0,0,0.7);
  padding: 5px;
  border-radius: 5px;
  background: white;
}
.flex-row{
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
  max-width: 1000px;
  width: 100%
}
.title{
  padding: 15px;
  color: black;
  font-size: 30px;
}
.black{
  color: black
}
.cont{
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%
}
</style>
