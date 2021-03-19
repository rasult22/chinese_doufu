<template>
  <div id="app">
    <!--container-->
<section class="container">

	<!--questionBox-->
	<div class="questionBox" id="app">

		<!-- transition -->
		<transition :duration="{ enter: 500, leave: 300 }" enter-active-class="animated zoomIn" leave-active-class="animated zoomOut" mode="out-in">

			<!--qusetionContainer-->
			<!-- <div class="questionContainer" v-if="false" v-bind:key="questionIndex"> -->
			<div class="questionContainer" v-if="questionIndex<quiz.questions.length" v-bind:key="questionIndex">

				<header>
					<h1 class="title is-6">Chinese Doufu</h1>
          <div class="logo">
            <img src="./assets/logo_1.jpg" width="60" class="logo_image">
          </div>
					<!--progress-->
					<div class="progressContainer">
						<progress class="progress is-info is-small" :value="(questionIndex/quiz.questions.length)*100" max="100">{{(questionIndex/quiz.questions.length)*100}}%</progress>
						<p>{{parseInt((questionIndex/quiz.questions.length)*100)}}% завершено</p>
					</div>
					<!--/progress-->
				</header>

				<!-- questionTitle -->
				<h2 class="titleContainer title">{{questionIndex + 1}}. {{ quiz.questions[questionIndex].text }}</h2>

				<!-- quizOptions -->
				<div class="optionContainer">
					<div class="option" v-for="(response, index) in quiz.questions[questionIndex].responses" @click="selectOption(index)" :class="{ 'is-selected': userResponses[questionIndex] == index}" :key="index">
						{{ index | charIndex }}. {{ response.text }}
					</div>
				</div>

				<!--quizFooter: navigation and progress-->
				<footer class="questionFooter">

					<!--pagination-->
					<nav class="pagination" role="navigation" aria-label="pagination">

						<!-- back button -->
						<a class="button" v-on:click="prev();" :disabled="questionIndex < 1">
                    Назад
                  </a>

						<!-- next button -->
						<a class="button" :class="(userResponses[questionIndex]==null)?'':'is-active'" v-on:click="next();" :disabled="questionIndex>=quiz.questions.length">
                    {{ (userResponses[questionIndex]==null)?'Пропустить':'Следующий' }}
                  </a>

					</nav>
					<!--/pagination-->

				</footer>
				<!--/quizFooter-->

			</div>
			<!--/questionContainer-->

			<!--quizCompletedResult-->
			<div v-if="questionIndex >= quiz.questions.length" v-bind:key="questionIndex" class="quizCompleted has-text-centered">
			<!-- <div v-if="true" v-bind:key="questionIndex" class="quizCompleted has-text-centered"> -->

				<!-- quizCompletedIcon: Achievement Icon -->
          <div class="logo">
            <img src="./assets/logo_1.jpg" width="60" class="logo_image">
          </div>
        <!-- </span> -->
        <h3>Chinese Doufu</h3>

				<!--resultTitleBlock-->
				<h2 class="title">
          {{score() > 20 ? '🥳 Отличный результат! Приходите к нам, и мы поможем вам улучшить его!' : '🤓 Ваш результат ниже среднего! Приходите к нам, мы поможем вам улучшить его!'}}
					<!-- You did {{ (score() > 7 ? 'an amazing' : (score() < 4 ? 'a poor' : 'a good')) }} job! -->
				</h2>
				<p class="subtitle">
					Вы набрали: {{ score() }} / {{ quiz.questions.length }}
				</p>
					<br>
					<a class="button" @click="restart()">Запустить снова <i class="fa fa-refresh"></i></a>
				<!--/resultTitleBlock-->
          <a href="https://www.instagram.com/chinese_doufu/" target="_blank">
            <div style="margin-bottom: 10px; margin-top: 90px;" class="instagram"></div>
            <span> Chinese Doufu </span>
          </a>
			</div>
			<!--/quizCompetedResult-->

		</transition>

	</div>
	<!--/questionBox-->

</section>
<!--/container-->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data () {
    return {
      quiz: quiz,
      questionIndex: 0,
      userResponses: userResponseSkelaton,
      isActive: false
    }
  },
  methods: {
    restart: function(){
      this.questionIndex=0;
      this.userResponses=Array(this.quiz.questions.length).fill(null);
    },
    selectOption: function(index) {
        this.$set(this.userResponses, this.questionIndex, index);
        //console.log(this.userResponses);
    },
    next: function() {
        if (this.questionIndex < this.quiz.questions.length)
          this.questionIndex++;
    },

    prev: function() {
        if (this.quiz.questions.length > 0) this.questionIndex--;
    },
    // Return "true" count in userResponses
    score () {
        let score = 0;
        for (let i = 0; i < this.userResponses.length; i++) {
          if (
              typeof this.quiz.questions[i].responses[
                this.userResponses[i]
              ] !== "undefined" &&
              this.quiz.questions[i].responses[this.userResponses[i]].correct
          ) {
              score = score + 1;
          }
        }
        return score;

        //return this.userResponses.filter(function(val) { return val }).length;
    }
  },
  filters: {
    charIndex (i) {
      return String.fromCharCode(97 + i);
    }
  }
}
let quiz = {
      user: "Dave",
      questions: [
         {
            text: "我哥哥很好 означает: ",
            responses: [
               { text: "Мы в полном порядке" },
               { text: "Он очень занят" },
               { text: "У моего старшего брата все хорошо", correct: true },
               { text: "Я не занят" }
            ]
         },
         {
            text: "Какой иероглиф читается как \"shī\":",
            responses: [
               { text: "是"},
               { text: "时" },
               { text: "师", correct: true },
               { text: "事" }
            ]
         },
         {
            text: "Какое предложение содержит общий вопрос?",
            responses: [
               { text: "这是什么?" },
               { text: "他是谁?" },
               { text: "那是书还是报?" },
               { text: "这是车, 是吗?", correct: true }
            ]
         },
         {
            text: "Какое предложение неотрицательное?",
            responses: [
               { text: "他还没来" },
               { text: "这不是木马" },
               { text: "她还在这儿", correct: true},
               { text: "身体也不好" }
            ]
         },
         {
            text: "Сколько черт в слове 汉语 ?",
            responses: [
               { text: "12" },
               {
                  text: "18"
               },
               { text: "14" , correct: true},
               { text: "16" }
            ]
         },
         {
            text:
               "Какая графема общая в иероглифах 忙, 怆, 沁, 恭, 密 ?",
            responses: [
               { text: "小" },
               { text: "心", correct: true },
               { text: "人" },
               { text: "刀" }
            ]
         },
         {
            text: "Подберите перевод к слову «учитель» : ",
            responses: [
               { text: "老虎" },
               { text: "老师", correct: true },
               {
                  text: "师傅"
               },
               { text: "教室" }
            ]
         },
         {
            text: "Найдите лишнее слово ",
            responses: [
               { text: "火" },
               { text: "车" },
               { text: "也", correct: true },
               { text: "手" }
            ]
         },
         {
            text:
               "Подберите перевод к 俄罗斯:",
            responses: [
               {
                  text: "все мои родственники"
               },
               { text: "родители" },
               { text: "русские" },
               { text: "Россия", correct: true }
            ]
         },
         {
            text: "Сколько черт в иероглифе 回 ?",
            responses: [
               { text: "2 " },
               { text: "3"},
               { text: "4" },
               { text: "6", correct: true  }
            ]
         },
         {
            text: "Вставьте недостающее слово:  __很多。",
            responses: [
               { text: "他 " },
               { text: "大", correct: true },
               { text: "没" },
               { text: "人" }
            ]
         },
         {
            text: "Иероглиф 见 означает: ",
            responses: [
               { text: "спать " },
               { text: "стоять"},
               { text: "говорить" },
               { text: "видеть", correct: true  }
            ]
         },
         {
            text: "Выберите нужный вопрос к части предложения между тире --他--是你爸爸的医生。",
            responses: [
               { text: "什么 " },
               { text: "谁的"},
               { text: "哪个" },
               { text: "谁", correct: true  }
            ]
         },
         {
            text: "Какой из этих глаголов модальный?",
            responses: [
               { text: "写 " },
               { text: "看"},
               { text: "吃" },
               { text: "能", correct: true  }
            ]
         },
         {
            text: "Подберите антоним к слову 新 ?",
            responses: [
               { text: "老 " },
               { text: "旧", correct: true},
               { text: "久" },
               { text: "歹"  }
            ]
         },
         {
            text: "В каком иероглифе 8 черт ?",
            responses: [
               { text: "青 ", correct: true  },
               { text: "食"},
               { text: "身" },
               { text: "音" }
            ]
         },
         {
            text: "Подберите перевод к «Что делает твоя младшая сестра?»",
            responses: [
               { text: "你妹妹爱什么？ " },
               { text: "我妹妹做什么?"},
               { text: "你妹妹做什么?", correct: true },
               { text: "你姐姐做什么?" }
            ]
         },
         {
            text: "Найдите лишнее слово:",
            responses: [
               { text: "白"},
               { text: "百", correct: true },
               { text: "红" },
               { text: "黑" }
            ]
         },
         {
            text: "Подберите перевод 请问 :",
            responses: [
               { text: "Входите, пожалуйста " },
               { text: "Позвольте узнать", correct: true },
               { text: "До свидания" },
               { text: "Спрашивайте, не стесняйтесь" }
            ]
         },
         {
            text: "Найдите ошибку:",
            responses: [
               { text: "子 – 3 черты", correct: true },
               { text: "系 – 7 черт"},
               { text: "母 – 6 черт" },
               { text: "豆 – 7 черт" }
            ]
         },
         {
            text: "Подберите перевод к «китайский язык» : ",
            responses: [
               { text: "说话 " },
               { text: "汉语", correct: true },
               { text: "中间" },
               { text: "语法"}
            ]
         },
         {
            text: "Вставьте недостающий иероглиф 你七点上课还是___点上课 ？",
            responses: [
               { text: "八", correct: true },
               { text: "人"},
               { text: "木" },
               { text: "西" }
            ]
         },
         {
            text: "Какое слово читается не одинаково с 雨 ?",
            responses: [
               { text: "玉" },
               { text: "鱼"},
               { text: "语" },
               { text: "月", correct: true }
            ]
         },
         {
            text: "Выберите правильный ответ на вопрос 你们的汉语教师姓什么？",
            responses: [
               { text: "不，他姓王。" },
               { text: "我们的教师叫云福。"},
               { text: "她姓张。", correct: true },
               { text: "是，汉语真难学。"}
            ]
         },
         {
            text: " Выберите нужный вопрос к части предложения, находящейся между тире  今天我妈妈买--英文报--。",
            responses: [
               { text: "谁"},
               { text: "什么", correct: true },
               { text: "怎么样?" },
               { text: "谁的" }
            ]
         },
         {
            text: "Подберите перевод «Они тоже не любят суп»",
            responses: [
               { text: "我们也不吃糖。" },
               { text: "你们还没喝茶。"},
               { text: "他们也不爱喝汤。", correct: true  },
               { text: "他们都不在食堂" }
            ]
         },
         {
            text: "Отрицательное предложение образуется с помощью",
            responses: [
               { text: "了 " },
               { text: "不", correct: true },
               { text: "呢" },
               { text: "还" }
            ]
         },
         {
            text: "Вставьте пропущенную в диалоге фразу 甲：我很好。___？ 乙：她也很好。",
            responses: [
               { text: " 妈妈吗？ " },
               { text: "你的父母呢?"},
               { text: "你呢?" },
               { text: "你妈妈呢?", correct: true  }
            ]
         },
         {
            text: "Найдите ошибку:",
            responses: [
               { text: "四位鸟", correct: true },
               { text: "两条鱼"},
               { text: "一块手表" },
               { text: "六支毛笔" }
            ]
         },
         {
            text: "Переведите 十七 ",
            responses: [
               { text: "Девятнадцать " },
               { text: "семьдесят"},
               { text: "10 июля" },
               { text: "семнадцать", correct: true  }
            ]
         },
         {
            text: "Ответьте на вопрос 你家有几口人?",
            responses: [
               { text: "我家没有人 " },
               { text: "我家有六口人", correct: true },
               { text: "我家不很大。" },
               { text: "你家有四口人" }
            ]
         },
         {
            text: "Найдите предложение с ошибкой:",
            responses: [
               { text: "他爸爸都四十岁了。" },
               { text: "你哥哥不有爱人", correct: true },
               { text: "我还没有车。" },
               { text: "我有点儿冷。" }
            ]
         },
         {
            text: "Какой из ответов читается как liuxuesheng?",
            responses: [
               { text: "旅行社 " },
               { text: "留学生", correct: true },
               { text: "六月底" },
               { text: "绿鞋子"}
            ]
         },
         {
            text: "Подберите ответ на вопрос 他是什么人?",
            responses: [
               { text: "他是我爸爸的朋友。", correct: true },
               { text: "我不认识她"},
               { text: "他很好。" },
               { text: "是汉学家。"}
            ]
         },
         {
            text: "Найдите общий ключ в данных иероглифах 魂, 魔, 魅 ",
            responses: [
               { text: "鬼", correct: true  },
               { text: "二"},
               { text: "儿" },
               { text: "臼" }
            ]
         },
         {
            text: "Что не читается как \"shi\"?",
            responses: [
               { text: "是 " },
               { text: "舍", correct: true},
               { text: "视" },
               { text: "师"  }
            ]
         },
         {
            text: "Подберите антоним к 来 ",
            responses: [
               { text: "上 " },
               { text: "去", correct: true },
               { text: "工" },
               { text: "坐" }
            ]
         },
         {
            text: "Вставьте недостающий иероглиф 朋 ",
            responses: [
               { text: "双 " },
               { text: "又" },
               { text: "友", correct: true },
               { text: "朋" }
            ]
         },
         {
            text: "Найдите ошибку :",
            responses: [
               { text: "他在吃第六个苹果。" },
               { text: "我是第二年级的学生。", correct: true },
               { text: "猫喝第三盘牛奶。" },
               { text: "请大家看第十课的课文。" }
            ]
         }
      ]
   }
const userResponseSkelaton = Array(quiz.questions.length).fill(null);
</script>

<style lang="scss">
.logo {
  border-radius: 100%;
  &_image {
    border-radius: 100%;
    object-fit: cover;
    margin-bottom: 10px;
  }
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
	width: 100%;
  display: flex;
  justify-content: center;
  color: #2c3e50;
  // margin-top: 60px;
}
$trans_duration: 0.3s;
$primary_color: #3D5AFE;

@import url("https://fonts.googleapis.com/css?family=Montserrat:400,400i,700");
@import url("https://fonts.googleapis.com/css?family=Open+Sans:400,400i,700");

body {
   font-family: "Open Sans", sans-serif;
   font-size: 14px;
	height: 100vh;

	background: #CFD8DC;
	
   /* mocking native UI */
   cursor: default !important; /* remove text selection cursor */
   user-select: none; /* remove text selection */
   user-drag: none; /* disbale element dragging */
	
	display: flex;
	align-items: center;
	justify-content: center;
}

.button {
   transition: $trans_duration;
}
.title,
.subtitle {
   font-family: Montserrat, sans-serif;
   font-weight: normal;
   font-size: 18px;
}
.animated {
   transition-duration: $trans_duration/2;
}

.container{
	// margin: 0 0.5rem;
}

.questionBox {
	max-width: 30rem;
	// width: 30rem;
	min-height: 30rem;
	background: #FAFAFA;
  position: relative;
  display: flex;
	border-radius: 0.5rem;
	overflow: hidden;
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
	
	header{
		background:rgba(0,0,0,0.025);
		padding: 1.5rem;
		text-align: center;
		border-bottom: 1px solid rgba(0,0,0,0.1);
		
		h1{
			font-weight: bold;
			margin-bottom: 1rem !important;
		}
    .progressContainer {
      width: 60%;
      margin: 0 auto;
      >progress{
        margin:0;
        border-radius: 5rem;
        overflow: hidden;
        border:none;
        
        color:$primary_color;
      &::-moz-progress-bar { background: $primary_color; }
      &::-webkit-progress-value { background: $primary_color; }
      }
      >p{
        margin:0;
        margin-top: 0.5rem;
      }
    }
	}
  .titleContainer {
    text-align: center;
    margin: 0 auto;
    padding: 1.5rem;
  }

   .quizForm {
      display: block;
      white-space: normal;

      height: 100%;
      width: 100%;

      .quizFormContainer {
         height: 100%;
         margin: 15px 18px;

         .field-label {
            text-align: left;
            margin-bottom: 0.5rem;
         }
      }
   }
   .quizCompleted {
    width: 100%;
    padding: 1rem;
    text-align:center;
    
    > .icon{
      color: #FF5252;
      font-size: 5rem;
      
      .is-active{
        color: #00E676;
      }
    }
   }
   .questionContainer {
      white-space: normal;
      height: 100%;
      width: 80vw;

      .optionContainer {
         margin-top: 12px;
         flex-grow: 1;
         .option {
          border-radius: 290486px;
          padding: 9px 18px;
          margin: 0 18px;
          margin-bottom: 12px;
          transition: $trans_duration;
          cursor: pointer;
          background-color: rgba(0, 0, 0, 0.05);
          color: rgba(0,0,0,0.85);
          border: transparent 1px solid;

          &:hover {
              background-color: rgba(0, 0, 0, 0.1);
          }
          &:active {
              transform: scaleX(0.9);
          }
          &.is-selected {
              border-color: #dce0f2;
              background-color: #dce0f2;
          }
        }
      }

      .questionFooter {
				background:rgba(0,0,0,0.025);
				border-top: 1px solid rgba(0,0,0,0.1);
         width: 100%;
         align-self: flex-end;

         .pagination {
            //padding: 10px 15px;
            margin: 15px 25px;
         }
      }
   }
}
.pagination{
	display: flex;
	justify-content: space-between;
}
.button{
	padding: 0.5rem 1rem;
	border: 1px solid rgba(0,0,0,0.25);
	border-radius: 5rem;
	margin: 0 0.25rem;
	
	transition:0.3s;
	
	&:hover{
		cursor: pointer;
		background: #ECEFF1;
		border-color:rgba(0,0,0,0.25);
	}
	&.is-active{
		background: $primary_color;
		color: white;
		border-color: transparent;
		
		&:hover{
			background: darken($primary_color,10%);
			
		}
	}
}

@media screen and (min-width: 769px) {
   .questionBox {
      align-items: center;
      justify-content: center;

      .questionContainer {
         display: flex;
         flex-direction: column;
      }
   }
}

@media screen and (max-width: 768px) {
   .sidebar {
      height: auto !important;
      border-radius: 6px 6px 0px 0px;
   }
}
$size: 10vmin;
$blur: .15*$size;
$inner: .7*$size;
$bw: .05*$size;
$diam: .3125*$size;
$off: .65*$diam;
.instagram {
  position: relative;
  margin: .5* $size auto 0;
  width: $size; height: $size;
  border-radius: 18.5%;
  box-shadow: 0 0 $blur rgba(#000, .5);
  background: radial-gradient(circle at 33% 100%, #FED373 4%, #F15245 30%, #D92E7F 62%, #9B36B7 85%, #515ECF);
  
  &:before, &:after {
    position: absolute;
    top: 50%; left: 50%;
    width: $inner; height: $inner;
    border: solid $bw #fff;
    transform: translate(-50%, -50%);
    content: '';
  }
  
  &:before { border-radius: 18.5%; }
  
  &:after {
    width: $diam; height: $diam;
    border-radius: 50%;
    box-shadow: $off (-$off) 0 (-.8*$off) #fff;
  }
}
</style>
