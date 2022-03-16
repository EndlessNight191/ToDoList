<template>
  {{local}}
    <h1 style="margin: 0 auto; color: white">My List</h1>
    <div v-if="show" class="dialog__visible__main">
        <input v-model="authObject.login" type="text" placeholder="Имя">
        <input v-model="authObject.password" :type="seePassword ? 'text' : 'password'" placeholder="Пороль" name="password">
        <input type="checkbox" id="checkbox" v-model="seePassword" />
        <label for="checkbox">{{ seePassword }}</label>
        <button @click="auth">Авторизоваться</button>
    </div>

    <div v-else class="main">
    <header>
        <div id="v-model-select" style="display: flex; align-items: center">
          <select v-model="selected">
            <option value="all">all</option>
            <option>memo</option>
            <option>meeting</option>
            <option>tusk</option>
          </select>
          <span>Выбрано: {{ selected }}</span>
        </div>
      <div class="filters">
        <img @click="inVisible" src="@/assets/Plus.svg" alt="" style="height: 25px; width: 25px">
        <img src="@/assets/lupa.svg" alt="" style="height: 25px; width: 25px" @click="visible = !visible">
        <transition name="slide-fade">
          <input v-if="visible" class="input__filters" type="text" placeholder="Поиск" v-model="searche">
        </transition>
        <div style="padding: 0px; margin: 0px">
          <img @click="isShowDropdown = !isShowDropdown" src="@/assets/filter.svg" alt="" style="height: 25px; width: 25px">
          <div v-if="isShowDropdown" class="selectDiv" @click.stop="isShowDropdown = false">
            <div class="optionDiv" @click="filterDefault">По умолчанию</div>
            <div class="optionDiv" @click="filterTime">Время</div>
            <div class="optionDiv" @click="filterDate">Дата</div>
          </div>
        </div>
      </div>
    </header>

      <myDialog :stroke="text" :element2="element3" @create="creaateElement" v-model:show="dialogVisible"/>
      <myDialog :stroke="text2" :element2="element2" @create="refactorElement" v-model:show="dialogVisible2"/>

      <transition-group name="list" >
        <elemItem @refactor="refactor" @deletes="deletes" :elem="elem" v-for="elem in elements2" :key="elem.index" class="list-complete-item"/>
      </transition-group>

  </div>

</template>

<script>
import elemItem from '@/components/elemItem.vue'
import myDialog from '@/components/myDialog.vue'

export default {
  name: 'App',
  data(){
    return{
      searche: '',
      element2: {
        prop: false
      },
      element3: {
        prop: false
      },
      isShowDropdown: false,
      visible: false,
      text: 'Добавить',
      text2: 'Изменить',
      dialogVisible: false,
      dialogVisible2: false,
      seePassword: false,
      show: false,
      selected: 'all',
      show__footer: true,
      authObject: {
          login: '',
          password: '',
      },
      trueAuth:{
          login: 'Vlad',
          password: '0000',
      },
      mainElements: [

      ],
      elements: [],
    }
  },
  components: {
    elemItem,
    myDialog,
  },
  methods: {
    auth(){
        if(this.authObject.login === this.trueAuth.login && this.authObject.password === this.trueAuth.password) {
          this.show = false
          this.show__foter = true
        }
    },
    add(){
        console.log(1)
    },
    filterTime(){
      let one = ''
      let two = ''

      this.elements.sort(function (a, b){

        for(let i=0; i<a.time.length; i++){
          if(a.time[i] !== ':'){
            one += a.time[i]
            one * 1
          }
        }

        for(let i=0; i<b.time.length; i++){
          if(b.time[i] !== ':'){
            two += b.time[i]
            two * 1
          }
        }

        if (one < two) {
          return -1;
        }
        if (one > two) {
          return 1;
        }
        // a должно быть равным b
        return 0;
      })
      this.isShowDropdown = false
    },
    filterDefault(){
      this.elements = this.mainElements
    },
    filterDate(){
      let yearOne = ''
      let mongthOne = ''
      let dayOne = ''
      let countOne = 0
      let yearTwo = ''
      let mongthTwo = ''
      let dayTwo = ''
      let countTwo = 0
      let one = 0
      let two = 0
      this.elements.sort(function (a, b){

        for(let i=a.date.length-1; i>=0; i--){
          if(a.date[i] !== '/'){
            countOne++
            if(countOne <= 4){
              yearOne += a.date[i]
            }else if(countOne >= 5 && countOne <= 6){
              mongthOne += a.date[i]
            }else if(countOne >= 7 && countOne <= 8){
              dayOne += a.date[i]
            }
          }
        }

        yearOne = ("00" + yearOne).slice(-2);
        mongthOne = ("00" + mongthOne).slice(-2);
        dayOne = ("00" + dayOne).slice(-2);

        for(let i=b.date.length-1; i>=0; i--){
          if(b.date[i] !== '/'){
            countTwo++
            if(countTwo <= 4){
              yearTwo += b.date[i]
            }else if(countTwo >= 5 && countTwo <= 6){
              mongthTwo += b.date[i]
            }else if(countTwo >= 7 && countTwo <= 8){
              dayTwo += b.date[i]
            }
          }
        }

        yearTwo = ("00" + yearTwo).slice(-2);
        mongthTwo = ("00" + mongthTwo).slice(-2);
        dayTwo = ("00" + dayTwo).slice(-2);

        one = yearOne + mongthOne + dayOne
        two = yearTwo + mongthTwo + dayTwo

        if (one < two) {
          return -1;
        }
        if (one > two) {
          return 1;
        }
        // a должно быть равным b
        return 0;
      })
    },
    inVisible(){
      this.dialogVisible = true
    },
    creaateElement(element){
      this.mainElements.push(element)
    },
    refactor(element){
      this.dialogVisible2 = true
      this.element2 = element
      this.element2.prop = true
    },
    refactorElement(element){
      for(let elem in this.mainElements){
        if(this.mainElements[elem].id === element.id){
            this.mainElements[elem] = element
        }
      }
    },
    deletes(id){
      for(let elem in this.mainElements){
        if(this.mainElements[elem].id === id){
          this.elements.splice(elem*1, 1)
        }
      }
    },
  },
  watch:{
    selected(newSelect, old){
       if(this.selected === 'all') {
         this.elements = this.mainElements
       }else{
         this.elements = this.mainElements.filter(function (item){
           return newSelect === item.types
         })
       }
    }
  },
  created() {
      if(localStorage.array) {
        this.mainElements = JSON.parse(localStorage.getItem('array'))
      }
    this.elements = this.mainElements
  },
  computed: {
    elements2() {
      if (this.searche) {
        return this.elements.filter(item => {
          return item.event.includes(this.searche);
        });
      }
      return this.elements
    },
    local(){
      if(this.mainElements.length > 0){
        localStorage.array = JSON.stringify(this.mainElements)
      }
      return ''
    }
  },
}
</script>

<style>
#app {
  font-family: 'Ubuntu', sans-serif;
  font-size: 20px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 0px;
  margin: 0px;
}

header{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.selectDiv{
  position: fixed;
  display: block;
  z-index:99999999;
  background-color: white;
  width: 150px;
  border-radius: 10px;
  right: 25%;
  border: 2px black solid;
}

.optionDiv{
  font-size: 19px;
  padding: 2px 4px;
  margin: 6px 0px;
  cursor: pointer;
}
.optionDiv:hover{
  background-color: gray;
  transition: .4s;
}

.filters{
  width: 60%;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

.main{
  padding: 20px;
  padding-bottom: 20px;
  margin: 0px auto;
  background-color: white;
  min-height: 80vh;
  width: 50vw;
  border-radius: 10px;
  position: relative;
}
html, body{
  background-color: #E8D1D1	;
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog__visible__main{
  width: 70%;
  height: 400px;
  display: flex;
  align-items: center;
  flex-direction: column;
  align-content: center;
}

input{
  width: 70%;
  height: 20px;
  margin-top: 40px;
  padding: 10px;
  color: gray;
}

.input__filters{
  width: 300px;
  padding: 5px;
  margin: 0px;
}

button{
  padding: 10px;
  border-radius: 10px;
  background-color: #E8D1D1;
  border: 2px solid white;
  cursor: pointer;
}


.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active,
.list-leave-active {
  transform: translateX(130px);
  transition: all .4s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}

.list-move{
  transition: transform 0.8s ease;
}

.slide-fade-enter-active {
  transition: all 0.8s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(300px);
  opacity: 0;
}
</style>
