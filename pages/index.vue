<template>
  <div class="container">
    <div class="compare">
      <div class="search">
        <BaseTextField
          :labelText="placeholder"
          :magnifier="magnifier"
          :trash="search.trash"
          :clearable="clearable"
          :inputFields="inputFields"
          v-for="(search, id) in inputFields"
          :key="id"
          v-model="search.value"
          @click="getResult(search.value)"

        />
        <InputAddButton :input-fields="inputFields" :max-input="maxInput"/>
      </div>
      <div>
          <BaseButton  @click="getResult('AllData')"></BaseButton>

      </div>
    </div>
    <div>

      <table class="compare-table" >
        <thead>
        <tr>
          <th class="download"></th>
          <th class="company-name" v-bind:key="result.id" v-for="result in results">{{result.name}}</th>
        </tr>
        </thead>
        <tbody>
        <tr  v-for="(category, id_cat) in col" :key="id_cat">
          <td class="category"  >{{category.name}}</td>
          <td class="content"  v-for="result in results">{{result[category.key]}}</td>
        </tr>
        </tbody>
      </table>
    </div>
    <nuxt-child></nuxt-child>
  </div>
</template>

<script>
import BaseTextField from "~/components/BaseTextField";
import BaseButton from "~/components/BaseButton";
import InputAddButton from "@/components/InputAddButton";
import axios from "axios"

export default {
  name: "compare",
  components: {
    BaseTextField,
    BaseButton,
    InputAddButton
  },
  methods:{
    getResult(value){

        axios.get("http://localhost:3000/rows/?name="+value)
          .then(res=>(this.results = res.data))
          .catch(err => console.log(err));
      console.log(value)
    }
  },

  data() {
    return {
      inputFields: [
        {
          id: 1,
          value: '',
          trash: false
        },
        {
          id: 2,
          value: '',
          trash: false
        }
      ],
      minInput: 2,
      maxInput: 4,
      placeholder: "Введите ИИН, БИН, ФИО, название компании",
      clearable: true,
      magnifier: true,
      results:[],
      col: [
        {
          id:2,
          name:"Руководитель",
          key:"director",
        },
        {
          id:3,
          name: "Учредитель",
          key:"founder"
        },
        {
          id:4,
          name: "Кол-во лет на рынке",
          key:"year"
        },
        {
          id:5,
          name:"Виды деятельности",
          key:"activities"
        },
        {
          id:6,
          name:"Размерность предприятия",
          key:"dimension"
        },
        {
          id:7,
          name:"Сумма налоговых отчислений за последние 5 лет, тг.",
          key:"year5"
        },
        {
          id:8,
          name:"Плательщик НДС",
          key: "nds"
        },
        {
          id:9,
          name:"Форма собственности",
          key: "ownership"
        },
        {
          id:10,
          name: "Наличие филиалов и редставительств",
          key: "branches"
        },
        {
          id:11,
          name: "Наличие сертификатов и авторских прав",
          key: "certificate"
        },
        {
          id:12,
          name: "Наличие лицензий",
          key: "license"
        },
        {
          id:13,
          name: "Участие в закупках",
          key: "procurement"
        },
        {
          id:14,
          name:"Заключенные гос. контракты",
          key: "contract"
        },
        {
          id:15,
          name: "История изменений",
          key:"history"
        },
        {
          id:16,
          name: "Благонадежность",
          key: "trust"
        },
        {
          id:17,
          name: "Участие в судебных делах",
          key: "courts"
        },
        {
          id:18,
          name:"Участие руководителя в других юридических лицах",
          key:"headParticipation"
        }
      ],
      keyword:''
    }
  },
}
</script>

<style lang="scss" scoped>
.compare {
  display: flex;
  flex-direction: row;
}

.search {
  display: grid;
  grid-template-columns: repeat(2, minmax(min-content, 501px));
  grid-gap: 8px;
  margin-right: 8px;
}

.title {
  font-size: 20px;
  line-height: 23px;
  color: #2c3e50;
  font-weight: 600;
  margin-top: 40px;
}
.content {
  p {
    padding: 10px 0;
    a {
      color: #007AFF;
    }
  }
}
.compare-table{
  width: 100%;
  border-spacing: 8px 4px;
  font-style: normal;
  font-size: 12px;
  line-height: 20px;
  color: #71757A;
  margin: 0;
}

.download{
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 9px
}

.download-icon{
  max-width: 205px;
  width: 205px;
  min-width: 165px;
  height: 44px;
}

.company-name{
  background: #2c3e501a;
  height: 42px;
  padding: 10px 16px;
  text-align: start;
  max-width: 419px;
  width: 419px;
  min-width: 235px;
}

.category{
  max-width:286px;
  min-width: 165px;
}

.content{
  color: #2C3E50;
}

td {
  max-width: 286px;
  min-width: 165px;
  border-radius: 2px;
  padding: 10px 16px 10px 16px;
  font-weight: normal;
}

tbody tr:nth-child(odd){
  background: #eef0f533;
}

tbody tr:nth-child(even){
  background: #eef0f580;
}

th{
  font-weight: normal;
}
</style>
