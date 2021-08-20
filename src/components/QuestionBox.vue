<template>
  <!-- <div>
  <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">
     <template #lead>
  {{currentQuestion.question}}
    </template>

    <hr class="my-4">
<b-list-group>
  <b-list-group-item
  v-for="(answer,index) in answers" 
  :key="index"
  @click="selectAnswer(index)"
  :class="answerClass(index)">
       {{answer}}
      </b-list-group-item>
</b-list-group>
    <b-button 
    variant="primary"
    @click="submitAnswer"
    :disabled="selectedIndex=== null || answered "
    >
    Submit
    </b-button>
    <b-button 
     @click="next"
     >
     Next
     </b-button>
  </b-jumbotron>
 
</div>-->
 <section class="u-clearfix u-section-1" id="sec-1d44">
      <div class="u-clearfix u-sheet u-sheet-1">
        <div class="u-clearfix u-expanded-width-lg u-expanded-width-md u-expanded-width-sm u-expanded-width-xl u-gutter-10 u-layout-wrap u-layout-wrap-1">
          <div class="u-gutter-0 u-layout">
            <div class="u-layout-row">
              <div class="u-container-style u-layout-cell u-size-30 u-layout-cell-1">
                <div class="u-container-layout u-container-layout-1">
                  <h2 class="u-text u-text-default u-text-1">QUESTIONS</h2>
                  <p class="u-text u-text-2">{{currentQuestion.question}}</p>
                  <a  class="u-border-2 u-border-grey-50 u-border-hover-grey-50 u-btn u-btn-round u-button-style u-hover-grey-50 u-none u-radius-50 u-text-hover-white u-btn-1"
                  @click="submitAnswer"
                  :disabled="selectedIndex=== null || answered "
                  >
                  Submit
                  </a>
                  <a  class="u-border-2 u-border-grey-50 u-border-hover-grey-50 u-btn u-btn-round u-button-style u-hover-grey-50 u-none u-radius-50 u-text-hover-white u-btn-2"
                  @click="next"
                  >
                  Next
                  </a>
                </div>
              </div>
              <div class="u-container-style u-grey-50 u-layout-cell u-size-30 u-layout-cell-2">
                <div class="u-container-layout u-valign-middle-xl u-valign-top-lg u-valign-top-md u-valign-top-sm u-valign-top-xs u-container-layout-2">
                  <div class="u-container-style u-group u-opacity u-opacity-90 u-radius-12 u-shape-round u-white u-group-1">
                    <div class="u-container-layout u-container-layout-3">
                      <a class="buttonn button1"
                     v-for="(answer,index) in answers" 
  :key="index"
  @click="selectAnswer(index)"
  :class="answerClass(index)">
       {{answer}}
                      </a>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    
   
</template>
<script>
import _ from 'lodash'
export default {
    props:{
        currentQuestion:Object,
        next:Function,
        increment:Function,
      
    },
    data(){
return{
    selectedIndex:null,
    shuffledAnswers:[],
    correctIndex:null,
    answered:false,
    image: {backgroundImage: "url(images/et-min.jpg"}
}
    },
    methods:{
selectAnswer(index){
    this.selectedIndex=index
},
shuffleAnswers(){
 let answers=[...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
 this.shuffledAnswers=_.shuffle(answers)
  this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
},
submitAnswer(){
    let isCorrect=false
    if(this.selectedIndex === this.correctIndex){
        isCorrect = true
    }
    this.answered=true
    this.increment(isCorrect)
 
},
 answerClass(index) {
      let answerClass = ''
      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = 'incorrect'
      }
      return answerClass
    }
    },
    computed:{
        answers(){
            let answers=[...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
        return answers
        }
    },
    watch:{
        currentQuestion: {
            immediate:true,
            handler(){
            this.selectedIndex=null 
            this.answered=false
            this.shuffleAnswers()
            }
          
        }
    }
}
</script>
<style scoped>

.list-group{
    margin-bottom:15px
}
.buttonn:hover{
    background: grey !important;
    cursor: pointer;

}
.newbtn{
    color:white !important;
    background:grey !important;
    width:15px !important;
}
.selected{
    background-color:#555555 !important;

}
.correct{
    background-color: lightgreen !important;
    
}
.incorrect{
    background-color: red !important;
}
.buttonn {
  background-color: white;
  border: 3px;
  border-color:grey;
  color: grey;
  padding: 20px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
  display:flex;
  flex-direction: column;
  flex-wrap:wrap;
  width:auto;
  margin: 15px 15px 15px 15px;
  position: relative;
}

.button1 {border-radius: 20px;}
</style>