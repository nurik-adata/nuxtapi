<template>
  <button
    @click="addNewSearchField"
    v-if="inputFields.length < maxInput"
  >
    + Добавить новый поиск
  </button>
</template>
<script>
export default {
  name:'InputAddButton',
  props:{
    inputFields:{
      type:Array,
    },
    maxInput:{
      type:Number,
      required: true
    }

  },
  methods:{
    addNewSearchField() {
      if(this.inputFields.length <= this.maxInput){
        this.inputFields.push({trash: true, value:""})
        console.log("aaa")
      }
    }
  }
}
</script>

<style lang="scss" scoped>
button{
  background-color: inherit;
  border: 1px dashed #007aff;
  border-radius: 2px;
  font-size: 16px;
  line-height: 19px;
  align-items: center;
  text-align: center;
  color: #007aff;
  max-width: 501px;
  width: 100%;
  margin-right: 8px;
  padding: 12px 16px;
  cursor: pointer;
}

</style>
