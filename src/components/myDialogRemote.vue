<template>
  <div class="dialog" v-if="show" @click.stop="hideDIalog">
    <div @click.stop class="dialog__content">
      <input placeholder="text" v-model="element.event" type="text" style="margin-bottom: 20px">
      <button @click="Create">{{ stroke }}</button>
      <div id="v-model-select" class="demo">
        <select v-model="element.types">
          <option value="all">all</option>
          <option>memo</option>
          <option>meeting</option>
          <option>tusk</option>
        </select>
        <span>Выбрано: {{ element.types }}</span>
        <Datepicker v-model="date" :format="format" />
        <h3 v-if="visible">Выберите все параметры!!!</h3>
      </div>
    </div>
  </div>
</template>

<script>
import Datepicker from "vue3-date-time-picker";
import 'vue3-date-time-picker/dist/main.css';
import {ref} from "vue";

export default {
  name: "myDialogRemote",
    components: { Datepicker },
    setup() {
      const date = ref(new Date());
      // In case of a range picker, you'll receive [Date, Date]
      const format = (date) => {
        var day = date.getDate();
        var month = date.getMonth() + 1;
        var year = date.getFullYear();

        if (day < 10 && month < 10){
          return `0${day}/0${month}/${year}`
        }else if(day < 10){
          return `0${day}/${month}/${year}`
        }else if(month < 10){
          return `${day}/0${month}/${year}`
        }

        return `${day}/${month}/${year}`;
      }

      return {
        date,
        format,
      }
    },
    data(){
      return{
        visible: false,
        element: this.element2,
      }
    },
    updated() {
      this.element = this.element2
    },
  props: {
      show:{
        type: Boolean,
        default: false,
      },
      element2:{
        type: Object,
      },
      stroke:{
        type: String
      }
    },
    methods: {
      hideDIalog(){
        this.$emit('update:show', false)
      },
      Create(){
        if(this.element.event && this.element.types) {
          console.log('REMOTE')
          this.element.date = this.format(this.date)
          let minut = this.date.getMinutes()
          let hours = this.date.getHours()
          let stroke = ''
          let stroke2 = ''
          if (minut.toString().length === 1) {
            stroke += 0
            stroke += minut
          } else {
            stroke += minut
          }

          if (hours.toString().length === 1) {
            stroke2 += 0
            stroke2 += hours
          } else {
            stroke2 += hours
          }

          stroke = ("00" + stroke).slice(-2);
          stroke2 = ("00" + stroke2).slice(-2);
          console.log(stroke2)
          console.log(stroke)
          this.element.time = `${stroke2}:${stroke}`
          this.$emit('update:show', false);
          this.element.id = Date.now();
          this.$emit('create', this.element);
          this.visible = false;
          this.element = {
            date: '',
            time: '',
            event: '',
            types: '',
          }
        }else{
          this.visible = true
        }
      },
  }
}
</script>

<style scoped>

</style>