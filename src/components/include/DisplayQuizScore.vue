<template>
  <div class="card col-md-6" style="padding: 30px">
    <div class="icontainer successicon" v-if="!this.showFail">
      <font-awesome-icon icon="check" />
    </div>

    <div class="icontainer failicon" v-if="this.showFail">
          <font-awesome-icon icon="times" />
    </div>

    <div class="score-container text-center" style="padding-top:30px">
        <h1 class="text-center">Your Score</h1>
        <h2 class="text-center">{{this.$props.quizScore}} %</h2>
    </div>

    <div class="actioncontainer text-center" style="padding-top:10px">
        <button class="btn btn-primary"
         @click="reloadPage"
        >
            Retake this quiz
        </button>
    </div>

  </div>
</template>

<script>
export default {
 name:'DisplayQuizScore',
 props:['quizScore'],
 data(){
   return{
       showFail:false
   }
 },
 methods:{
     checkIconShown(){
         let score = this.$props.quizScore
         if(score < 50){
             this.showFail = true
         }
     },
     reloadPage(){
         location.reload()
     }
 },
 mounted(){
     this.checkIconShown()
 }
}
</script>

<style scoped>

.icontainer{
    width:150px;
    height:150px;
    margin:auto;
    border-radius:50%;
    border:5px solid;
    display: flex;
    justify-content:center;
    align-items:center;

}
 .fa-check ,.fa-times{
     font-size:60px !important;
 }


 .successicon{
     border-color: forestgreen;
 }

 .failicon{
     border-color:crimson
 }

 .fa-check{
     color:forestgreen
 }
 .fa-times{
     color:crimson
 }
</style>