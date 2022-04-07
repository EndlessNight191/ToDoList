<template>
  <div class="infoTime">
    <div>Упоминание о задании</div>
    <div>{{element.event}}</div>
    <div>{{ element.time }}</div>
  </div>
</template>

<script>
export default {
  name: "MyInfoTime",
  props: {
    element: Object
  }
}
</script>

<style scoped>
  .infoTime{
    position: absolute;
    background-color: white;
    height: 100px;
    width: 20%;
    top: 4%;
    right: 78%;
    border-radius: 15px;
  }
</style>