<template>
    <div class="infoMain">
      <div class="infoBlock">
        <div class="progress">
          <h2>Поставленно целей</h2>
          <progress max="100" :value="percentHundred"></progress>
          <div class="progress-value"></div>
          <div class="progress-bg"><div class="progress-bar"></div></div>
          <div style="display: flex; justify-content: space-between; align-items: center; margin-top: 0.5rem">
            <div style="font-size: 22px; color: #ac92ec">{{procent}}</div>
            <img src="@/assets/ToDo.png" alt="" style="height: 40px; width: 40px">
          </div>
        </div>
      </div>
      <div class="infoBlock">
        <div class="progress">
          <h2>Выполнено целей</h2>
          <progress max="100" :value="percentHundred2"></progress>
          <div class="progress-value"></div>
          <div class="progress-bg"><div class="progress-bar"></div></div>
          <div style="display: flex; justify-content: space-between; align-items: center; margin-top: 0.5rem">
            <div style="font-size: 22px; color: #ac92ec">{{procent2}}</div>
            <img src="@/assets/well-done.png" alt="" style="height: 40px; width: 40px">
          </div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: "MyInformate",
  props:{
    percentHundred:{
      type: Number,
    },
    percentHundred2:{
      type: Number,
    },
    procent: {
      type: Number,
      default: 0
    },
    procent2: {
      type: Number,
      default: 0
    },
  },
  data(){
    return{

    }
  }
}
</script>

<style lang="scss">
.infoMain{
  display: flex;
  justify-content: space-between;
  margin-bottom: 2rem;
}

.infoBlock{
  width: 45%;
  min-height: 150px;
  height: 30%;
  border-radius: 10px;
  background-color: white;
}

@for $i from 0 through 100 {
  .progress progress[value="#{$i}"]
  {
    & + .progress-value:before { content : '#{$i}%' }
    & ~ .progress-bg .progress-bar { width: $i * 1% }
  }
}

.progress
{
  font-size: 14px;
  position: relative;
  overflow: hidden;
  width: 80%;
  margin: 10px auto;
  progress
  {
    position: absolute;
    width: 0;
    height: 0;
    overflow: hidden;
    left: -777px;
  }
}

.progress-value
{
  color: #333;
  display: block;
  line-height: 21px;
  text-align: center;
}

.progress-bg
{
  background: #e6e9ed;
  position: relative;
  height: 8px;
  border-radius: 5px;
  overflow: hidden;
}

@-webkit-keyframes progress_bar {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: -40px 0;
  }
}
@-moz-keyframes progress_bar {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: -40px 0;
  }
}
@keyframes progress_bar {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: -40px 0;
  }
}

.progress-bar
{
  overflow: hidden;
  background: #ac92ec;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  width: 0;
  -webkit-transition: width 1s linear;
  -moz-transition: width 1s linear;
  -ms-transition: width 1s linear;
  transition: width 1s linear;
  &:after
  {
    -webkit-animation: progress_bar 0.8s linear infinite;
    -moz-animation: progress_bar 0.8s linear infinite;
    -ms-animation: progress_bar 0.8s linear infinite;
    -o-animation: progress_bar 0.8s linear infinite;
    animation: progress_bar 0.8s linear infinite;

    background-image: -webkit-linear-gradient(45deg,rgba(255,255,255,.15) 25%,transparent 25%,transparent 50%,rgba(255,255,255,.15) 50%,rgba(255,255,255,.15) 75%,transparent 75%,transparent);
    background-image: -o-linear-gradient(45deg,rgba(255,255,255,.15) 25%,transparent 25%,transparent 50%,rgba(255,255,255,.15) 50%,rgba(255,255,255,.15) 75%,transparent 75%,transparent);
    background-image: linear-gradient(45deg,rgba(255,255,255,.15) 25%,transparent 25%,transparent 50%,rgba(255,255,255,.15) 50%,rgba(255,255,255,.15) 75%,transparent 75%,transparent);
    -webkit-background-size: 40px 40px;
    background-size: 40px 40px;
    position: absolute;
    content: '';
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
}
</style>