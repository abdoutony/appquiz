<template>
   <div class="col-md-6 d--f jc--c">
       <div class="text-center">
            <h5>Timer</h5>
            <p>{{this.$props.timer}}</p>
       </div>
   </div>
</template>

<script>
export default {
  name:"Timer",
  props:['timer']
}
</script>

<style>

</style>