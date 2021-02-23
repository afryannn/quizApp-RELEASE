<template>
  <div class="container-app">
    <div class="container-quiz">
      <div class="quiz-header">
        <h1>Quiz Form</h1>
      </div>

      <div class="step-progress" :style="{'width': progress + '%'}"></div>

      <div
        class="quiz-main"
        v-for="(element, index) in questions.slice(a, b)"
        :key="index"
        v-show="quiz"
      >
        <div class="box-question">
          <h2>Question{{ b }}/{{ questions.length }}</h2>
          <p>{{ element.question }}</p>
        </div>
        <div class="box-suggestions">
          <ul>
            <li
              v-for="(item, index) in element.suggestions"
              :key="index"
              :class="select ? check(item) : ''"
              @click="selectResponse(item)"
            >
              {{ item.suggestion }}
              <div class="fas fa-check" v-if="select ? item.correct:''"></div>
              <div class="fas fa-times" style="color:#ec0101;" v-if="select ? !item.correct:''"></div>
            </li>
          </ul>
        </div>
      </div>
      <div class="box-score" v-if="score_show">
        <h2>Your Score is</h2>
        <h2>{{ score }}/{{ questions.length }}</h2>
        <div class="btn-restart">
          <button @click="restartQuiz">Restart<i class="fas fa-sync-alt"></i></button>
        </div>
      </div>
      <div class="quiz-footer">
        <div class="box-button" v-if="progress < 100">
          <button @click="skipQuestion" :style="next ? 'background-color:#686d76' : ''">Skip</button>
          <button @click="nextQuestion" :style="!next ? 'background-color:#120078' : ''">Next</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      questions: [
        {
          question: "inside which HTML element do we put the javascript?",
          suggestions: [
            { suggestion: "<script>", correct: true },
            { suggestion: "<js>" },
            { suggestion: "<javascript>" },
            { suggestion: "<scripting>" },
          ],
        },
        {
          question:
            "What is the correct syntax for referring to an external script called 'xxx.js' ",
          suggestions: [
            { suggestion: '<script href="xxx.js">' },
            { suggestion: '<script name="xxx.js">' },
            { suggestion: '<script src="xxx.js">', correct: true },
            { suggestion: '<script id="xxx.js">' },
          ],
        },
      ],
      a: 0,
      b: 1,
      select: false,
      score: 0,
      quiz: true,
      score_show: false,
      next:false,
      progress:0
    };
  },
  methods: {
    selectResponse(e) {
      this.select = true;
      if (e.correct) {
        this.score++;
      }
    },
    check(status) {
      if (status.correct) {
        return "correct";
      } else {
        return "incorrect";
      }
    },
    nextQuestion() {
      if(this.next){
        return;
      }
      this.progress = this.progress + (100/this.questions.length);
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
        this.select = false;
        this.next = false;
      }
    },
    skipQuestion(){
      if(this.questions.length - 1 == this.a){
        this.score_show = true;
        this.quiz = false;
      }else{
        this.a++;
        this.b++;
      }
    },
    restartQuiz(){
      Object.assign(this.$data, this.$options.data())
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
