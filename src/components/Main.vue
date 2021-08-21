<template>
   <div class="container">
        <div class="col-md-12">
           <div  class="row">
               <div class="d--f jc--c">

                    <DisplayQuizScore :quizScore='this.quizScore'
                     v-if='displayquizscore'
                     />

                  <div class="card col-md-9"
                   v-if="!displayquizscore"
                  >
                      <div class="card-body">
                         <div class="row d--f jc--c">
                            <div class="text-center">
                                 <h1 class="text-center">
                                     Take your quiz now
                                 </h1>
                                 <p>you have 10 minuts to finish the quiz</p>
                            </div>
                         </div>

                         <div class="row question-r-t">
                                <QuestionsRemaining :questionsremaining="questionsremaining" />
                                <Timer :timer="this.timer" />
                         </div>

                         <div>
                             <QuizInfo :category="this.category" :difficulty="this.difficulty"
                              :question="this.question"/>
                             <form>
                                 <div class="d--f jc--c">
                                    <div class="inputcontainer">
                                       <input type="radio" name="answer" id="answer" value="True"
                                       v-model="form.answer"> True
                                    </div>
                                    <div class="inputcontainer">
                                       <input type="radio" name="answer"
                                        id="answer" value="False"
                                        v-model="form.answer"> False
                                    </div>

                                 </div>

                                 <div class="row" style="padding-top:30px;padding-bottom:10px">
                                    <div class="col-md-6 text-center">
                                          <button class="btn btn-danger"
                                          @click.prevent="perviousQuestion"
                                          :disabled="isPerviousButtonDisabled"
                                          >Pervious Question</button>
                                    </div>
                                    <div class="col-md-6 text-center">
                                          <button class="btn btn-primary"
                                           @click.prevent="setAnswer"
                                           v-if="!quizFinished"
                                          >Next Question</button>
                                          <button class="btn btn-success"
                                          v-if="quizFinished"
                                          @click.prevent="calculateQuizScore"
                                          >
                                          Finish and see result
                                          </button>
                                    </div>

                                 </div>
                             </form>
                         </div>

                      </div>
                  </div>
               </div>
           </div>
        </div>
   </div>
</template>

<script>
import Vue from 'vue';
import Form from 'vform';
import QuestionsRemaining from './include/QuestionsRemaining.vue';
import Timer from './include/Timer.vue'
import QuizInfo from './include/QuizInfo.vue'
import axios from 'axios';
import DisplayQuizScore  from './include/DisplayQuizScore.vue';
export default {
    name:'Main',
    components:{
        QuestionsRemaining,Timer,QuizInfo,DisplayQuizScore

    },
    data(){
       return{
           questionindex:0,
           quiz:[],
           category:'',
           difficulty:'',
           question:'',
           quizFinished:false,
           form:new Form({
              answer:'',
           }),
           answers:[],
           quizScore:0,
           displayquizscore:false,
           isPerviousButtonDisabled:false,
           timer:600

       }
    },
    methods:{
        getQuiz(){
            axios.get('https://opentdb.com/api.php?amount=10&category=11&difficulty=easy&type=boolean')
            .then((response) =>{
                this.quiz = response.data['results']
                this.category = this.quiz[this.questionindex].category
                this.difficulty = this.quiz[this.questionindex].difficulty
                this.question = this.quiz[this.questionindex].question


                console.log(this.quiz)
            })
        },
        setQuizFinished(){
            this.quizFinished = true
        },
        setdisplayquizscoretrue(){
           this.displayquizscore  = true
        },
        setAnswer(){
            Vue.set(this.answers,this.answers.length,this.form.answer)
            if(this.questionindex == (this.quiz.length - 1)){
                this.setQuizFinished()
            }else{
                this.questionindex +=1
                this.category = this.quiz[this.questionindex].category
                this.difficulty = this.quiz[this.questionindex].difficulty
                this.question = this.quiz[this.questionindex].question
                this.form.reset()
            }

            if(this.questionindex >0){
                this.enablePerviousButton()
            }
        },
        calculateQuizScore(){
            for(let i =0;i< this.answers.length;i++){
                if(this.answers[i] == this.quiz[i].correct_answer){
                    this.quizScore +=10
                }
            }
            console.log(this.quizScore)
            this.setdisplayquizscoretrue()
        },
        disablePerviousButton(){
         this.isPerviousButtonDisabled = true
        },
        enablePerviousButton(){
            this.isPerviousButtonDisabled = false
        },
        perviousQuestion(){
            if(this.questionindex > 0){
                 this.questionindex -=1;
                 this.category = this.quiz[this.questionindex].category
                this.difficulty = this.quiz[this.questionindex].difficulty
                this.question = this.quiz[this.questionindex].question
                this.answers.pop()

            }
            if(this.questionindex ==0){
                this.disablePerviousButton()
            }
        },
        setTimer(){
            setInterval(function(){
                this.timer  = this.timer -1
                if(this.timer == 0){
                    this.calculateQuizScore()
                }
            }.bind(this),1000)
        }
    },
    mounted(){
        this.getQuiz()
        this.disablePerviousButton()
        this.setTimer()
    },
    computed:{
        questionsremaining(){
            return this.quiz.length - this.questionindex
        }
    }
}
</script>

<style>

</style>