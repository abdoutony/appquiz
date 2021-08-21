<template>
    <div class="col-md-6 d--f jc--c">
        <div class="text-center">
            <h5>Questions Remaining</h5>
            <p>{{this.$props.questionsremaining}}</p>
        </div>
    </div>
</template>

<script>
export default {
  props:['questionsremaining'],
  name:'QuestionsRemaining'
}
</script>

<style>

</style>