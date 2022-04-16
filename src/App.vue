<template>
  {{local}}
    <div v-if="show" class="dialog__visible__main">
        <input v-model="authObject.login" type="text" placeholder="Имя">
        <input v-model="authObject.password" :type="seePassword ? 'text' : 'password'" placeholder="Пороль" name="password">
        <input type="checkbox" id="checkbox" v-model="seePassword" />
        <label for="checkbox">{{ seePassword }}</label>
        <button @click="auth">Авторизоваться</button>
    </div>

    <div v-else>
      <transition name="InfoTime">
        <MyInfoTime v-if="showInfoTime" :element="itemInfoTime"></MyInfoTime>
      </transition>
      <h1 style="margin: 0.5rem auto; color: white">My List</h1>

      <MyInformate v-model:procent="procent" v-model:procent2="procent2" v-model:percentHundred="percent1" v-model:percentHundred2="percent2" />

    <div class="main">
    <header>
        <div id="v-model-select" style="display: flex; align-items: center">
          <select v-model="selected">
            <option value="all">all</option>
            <option>memo</option>
            <option>meeting</option>
            <option>tusk</option>
          </select>
          <span style="margin-left: 10px">Выбрано: <span style="color: #ac92ec; font-size: 26px">{{ selected }}</span></span>
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
            <div class="optionDiv" @click="filterDone">Выполненые</div>
            <div class="optionDiv" @click="filterNotCompleted">Поставленные</div>
          </div>
        </div>
      </div>
    </header>

      <transition-group name="list" tag="div" class="instruments">
        <elemItem @refactor="refactor" @deletes="deletes" @changeDone="changeDone" :elem="elem" v-for="elem in elements2" :key="elem.id"/>
      </transition-group>

  </div>
      <my-dialog-create :stroke="text" @create="creaateElement" v-model:show="dialogVisible"/>
      <my-dialog-remote :stroke="text2" v-model:element2="element2" @create="refactorElement" v-model:show="dialogVisible2"/>
    </div>

</template>

<script>
import elemItem from '@/components/elemItem.vue';
import MyInformate from '@/components/MyInformate.vue';
import MyInfoTime from "@/components/MyInfoTime";
import myDialogRemote from '@/components/myDialogRemote.vue';
import myDialogCreate from '@/components/myDialogCreate.vue';


export default {
  name: 'App',
  data() {
    return {
      showInfoTime: false,
      itemInfoTime: '',
      percent1: 0,
      percent2: 0,
      searche: '',
      element2: {},
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
      trueAuth: {
        login: 'Vlad',
        password: '0000',
      },
      mainElements: [],
      elements: [],
    }
  },
  components: {
    elemItem,
    MyInformate,
    MyInfoTime,
    myDialogCreate,
    myDialogRemote,
  },
  methods: {
    changeDone(element) {
      let count = 0
      for (let elem of this.mainElements) {
        if (elem.id === element.id) {
          this.mainElements[count].done = element.done
        }
        count++
      }

      let plusCount = 0
      let minusCount = 0
      for (let ThisElement of this.mainElements) {
        if (!ThisElement.done) {
          plusCount++
        } else {
          minusCount++
        }
      }
      this.percent1 = Math.floor((plusCount * 100) / this.mainElements.length)
      this.percent2 = Math.floor((minusCount * 100) / this.mainElements.length)
    },
    auth() {
      if (this.authObject.login === this.trueAuth.login && this.authObject.password === this.trueAuth.password) {
        this.show = false
        this.show__foter = true
      }
    },
    add() {
      console.log(1)
    },
    filterTime() {
      this.elements.sort(function (a, b) {
        if (a.time < b.time) {
          return -1;
        }
        if (a.time > b.time) {
          return 1;
        }
        // a должно быть равным b
        return 0;
      })
      this.isShowDropdown = false
    },
    filterDefault() {
      this.elements = this.mainElements
      this.elements.sort(function (a, b) {
        if (a.id < b.id) {
          return -1;
        }
        if (a.id > b.id) {
          return 1;
        }
        // a должно быть равным b
        return 0;
      })
      this.isShowDropdown = false
    },
    filterDone() {
      this.elements = this.mainElements.filter(item => item.done === true)
    },
    filterNotCompleted() {
      this.elements = this.mainElements.filter(item => item.done === false)
    },
    filterDate() {
      this.elements.sort(function (a, b) {
        if (a.date < b.date) {
          return -1;
        }
        if (a.date > b.date) {
          return 1;
        }
        // a должно быть равным b
        return 0;
      })
    },
    inVisible() {
      this.dialogVisible = true
    },
    creaateElement(element) {
      this.mainElements.push(element)

      let plusCount = 0
      let minusCount = 0
      for (let ThisElement of this.mainElements) {
        if (!ThisElement.done) {
          plusCount++
        } else {
          minusCount++
        }
      }
      this.percent1 = Math.floor((plusCount * 100) / this.mainElements.length)
      this.percent2 = Math.floor((minusCount * 100) / this.mainElements.length)
    },
    refactor(element) {
      this.element2 = element
      this.dialogVisible2 = true
    },
    refactorElement(element) {
      for (let elem in this.mainElements) {
        if (this.mainElements[elem].id === element.id) {
          this.mainElements[elem] = element
        }
      }
      this.element2 = {}
    },
    deletes(id) {
      for (let elem in this.mainElements) {
        elem = Number(elem)
        if (this.mainElements[elem].id === id) {
          this.mainElements.splice(elem, 1)
          break
        }
      }

      let plusCount = 0
      let minusCount = 0
      for (let element of this.mainElements) {
        if (!element.done) {
          plusCount++
        } else {
          minusCount++
        }
      }
      this.percent1 = Math.floor((plusCount * 100) / this.mainElements.length)
      this.percent2 = Math.floor((minusCount * 100) / this.mainElements.length)
    },
  },
  watch: {
      selected(newSelect, old) {
        if (this.selected === 'all') {
          this.elements = this.mainElements
        } else {
          this.elements = this.mainElements.filter(function (item) {
            return newSelect === item.types
          })
        }
      },
    },
    created() {
      if (localStorage.array) {
        this.mainElements = JSON.parse(localStorage.getItem('array'))
      }
      this.elements = this.mainElements

      if(localStorage.percent1 || localStorage.percent2){
        this.percent1 = JSON.parse(localStorage.getItem('percent1'))
        this.percent2 = JSON.parse(localStorage.getItem('percent2'))
      }

      setInterval(() => {
        let time = new Date().toLocaleString()
        time = time.split(',')
        let date = time[0]
        let timeWatch = time[1].slice(1)
        const date1 = new Date(`${date}, ${timeWatch}`);
        this.elements.map(item => {
          const date2 = new Date(`${item.date}, ${item.time}`);
          const difference = date2.getTime() - date1.getTime();
          let minutes = Math.floor(difference / 60000);
          if ((minutes <= 60 && minutes >= 59) || (minutes <= 30 && minutes >= 29)) {
            this.itemInfoTime = item
            this.showInfoTime = true
            setInterval(() => {
              this.itemInfoTime = ''
              this.showInfoTime = false
            }, 6000)
          } else {
            this.itemInfoTime = ''
            this.showInfoTime = false
          }

        })

      }, 60000);
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
      local() {
        if (this.mainElements.length > -1) {
          localStorage.array = JSON.stringify(this.mainElements)
        }
        localStorage.percent1 = JSON.stringify(this.percent1)
        localStorage.percent2 = JSON.stringify(this.percent2)
        return ''
      },
      procent() {
        let count = 0
        for (let element of this.mainElements) {
          if (!element.done) {
            count++
          }
        }

        return count
      },
      procent2() {
        let count = 0
        for (let element of this.mainElements) {
          if (element.done) {
            count++
          }
        }

        return count
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

@media (max-width: 1150px) {
  header{
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    margin-bottom: 20px;
  }

  .filters{
    width: 450px;
    display: flex;
    align-items: center;
    margin-left: 40%;
  }
}

.selectDiv{
  position: absolute;
  display: block;
  z-index:99999999;
  background-color: white;
  width: 150px;
  border-radius: 10px;
  border: 2px black solid;
  right: 3%;
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
  border-radius: 10px;
  border: 2px solid black;
  height: 20px;
  margin-top: 40px;
  padding: 10px;
  color: black;
  font-size: 16px;
  font-weight: 600;
  transition: .5s;
}

input:focus{
  outline:none;
  width: 70%;
  border-radius: 10px;
  border: 2px solid #ac92ec;
  height: 20px;
  margin-top: 40px;
  padding: 10px;
  color: #ac92ec;
  font-size: 16px;
  font-weight: 600;
  transition: .5s;
}

.input__filters{
  width: 300px;
  padding: 5px;
  margin: 0px;
  border-radius: 10px;
  border: 2px solid black;
  height: 20px;
  color: black;
  font-size: 16px;
  font-weight: 600;
  transition: .5s;
}

.input__filters:focus{
  outline:none;
  width: 300px;
  padding: 5px;
  margin: 0px;
  border-radius: 10px;
  border: 2px solid #ac92ec;
  height: 20px;
  color: #ac92ec;
  font-size: 16px;
  font-weight: 600;
  transition: .5s;
}

button{
  padding: 10px;
  border-radius: 10px;
  background-color: #ac92ec;
  border: 2px solid white;
  cursor: pointer;
  margin-left: 10px;
  font-size: 16px;
  color: black;
  font-weight: 600;
  transition: .5s;
}

button:hover{
  padding: 10px;
  border-radius: 10px;
  background-color: white;
  border: 2px solid #ac92ec;
  cursor: pointer;
  margin-left: 10px;
  font-size: 16px;
  color: #ac92ec;
  font-weight: 600;
  transition: .5s;
}

select{
  font-weight: 600;
  height: 40px;
  cursor: pointer;
  border: 2px gray solid;
  color: #ac92ec;
  transition: .5s;
}

select:hover{
  font-weight: 600;
  height: 40px;
  cursor: pointer;
  border: 2px #ac92ec solid;
  color: #ac92ec;
}

option{
  display: flex;
  justify-content: center;
}

@media (max-width: 560px) {
  .main{
    padding: 20px;
    padding-bottom: 20px;
    margin: 0px auto;
    background-color: white;
    min-height: 80vh;
    width: 80vw;
    border-radius: 10px;
    position: relative;
  }

  .input__filters{
    width: 175px;
  }

  .input__filters:focus{
    width: 175px;
  }
}

.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active,
.list-leave-active {
  transform: translateX(0px);
  transition: all .4s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(130px);
  position: relative;
}

.list-move{
  transition: transform 0.8s ease;
}

.slide-fade-enter-active {
  transition: all 0.8s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
  transform: translateX(300px);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(300px);
  opacity: 0;
}

.InfoTime-enter-active,
.InfoTime-leave-active {
  transform: translateY(-3%);
  transition: 1s;
  opacity: 1;
}

.InfoTime-enter-from,
.InfoTime-leave-to {
  position: absolute;
  transform: translateY(150px);
  opacity: 0;
  transition: 1s;
}
</style>
