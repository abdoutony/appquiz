<template>
   <div class="text-center">
      <div style="padding-top:10px; padding-bottom:10px;">
          <span style="margin-left:5px;margin-right:5px">
                <span><b>Category: </b></span>
                <span>{{this.$props.category}}</span>
          </span>

           <span style="margin-left:5px;margin-right:5px">
                <span><b>Difficulty: </b></span>
                <span>{{this.$props.difficulty}}</span>
          </span>

          <div class="questionblock mt-4">
              <span>
                  {{this.$props.question}}
              </span>
          </div>


      </div>
   </div>
</template>

<script>
export default {
  props:['category','difficulty','question'],
  name:'QuizInfo'
}
</script>

<style>

</style>