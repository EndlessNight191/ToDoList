<template>

      <div class="main__element">
      <button class="white" :class="{white2: isDone}" @click="switching"></button>
      <div style="display: flex; flex-direction: column; width: 100%" >
        <div style="width: 35%; display: flex; justify-content: space-between; font-size: 15px;" :class="{ doneClass: isDone }">
          <strong>{{elem.date}}</strong>
          <strong>{{elem.time}}</strong>
          <strong>{{elem.types}}</strong>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; width: 100%">
          <strong :class="{ doneClass: isDone }">{{elem.event}}</strong>
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
        isDone: false
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
      switching(){
        this.isDone = !this.isDone
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
      padding: 6px;
      border: 2px solid red;
      background-color: white;
      border-radius: 50%;
      margin-right: 10px;
      display: block;
    }

    .white2{
      padding: 6px;
      border: 2px solid red;
      background-color: red;
      border-radius: 50%;
      margin-right: 10px;
      display: block;
    }

    .doneClass{
      text-decoration: line-through;
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

</style>