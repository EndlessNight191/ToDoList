<template>

      <div class="main__element">
      <button class="white" @click="changeDone">
        <transition name="done">
          <div v-if="mainElem.done" class="isDone"></div>
        </transition>
      </button>
      <div style="display: flex; flex-direction: column; width: 100%; min-width: 100px" >
        <div class="infoItem" :class="{ doneClass: mainElem.done }">
          <div>{{elem.date}}</div>
          <div>{{elem.time}}</div>
          <div>{{elem.types}}</div>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; width: 100%">
          <strong class="doneClassMain" :class="{ doneClass: mainElem.done }">{{elem.event}}</strong>
          <div style="display: flex; align-items: center;">
          </div>
        </div>
      </div>
        <img src="@/assets/karandash.svg" alt="" @click="refactor" style="height: 20px; width: 20px">
        <img src="@/assets/korzina.svg" alt="" @click="deletes" style="height: 20px; width: 20px">
    </div>

</template>
<script>
export default {
    data(){
      return{
        mainElem: this.elem
      }
    },
    props:  {
        elem: {
            type: Object,
        }
    },
    methods: {
      refactor(){
          this.$emit('refactor', this.elem)
      },
      deletes(){
          this.$emit('deletes', this.elem.id)
      },
      changeDone(){
          this.mainElem.done = !this.mainElem.done
          this.$emit('changeDone', this.mainElem)
      }
    }
}
</script>
<style>

    .main__element{
      padding: 5px 20px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.25), 0 5px 10px rgba(0,0,0,0.22);
      width: 90%;
      align-items: center;
      display: flex;
      margin: 10px auto;
      border-radius: 8px;
    }

    .white{
      padding: 7px;
      width: 12px;
      height: 12px;
      border: 2px solid #ac92ec;
      background-color: white;
      border-radius: 50%;
      margin-right: 10px;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .isDone{
      padding: 5px 5px;
      background-color: #ac92ec;
      border-radius: 50%;
    }


    .doneClass{
      text-decoration: line-through;
    }

    .doneClassMain{
      max-width: 60%;
    }

    img{
      margin: 0px 5px;
      border-radius: 5px;
      cursor: pointer;
      padding: 5px;
    }

    img:hover{
      background-color: gray;
      transition: .3s;
    }

    .infoItem{
      width: 50%;
      display: flex;
      justify-content: space-between;
      font-size: 15px;
    }
    
    @media (max-width: 1100px) {
      .infoItem{
        width: 100%;
      }
    }

    .done-enter-active,
    .done-leave-active {
      opacity: 1;
      transition: all .5s ease;
    }
    .done-enter-from,
    .done-leave-to {
      opacity: 0;
      transition: all .5s ease;
    }


</style>