<template>
  <div id="app">
    <h1 class="title-app">QUIZ</h1>
    <main>
      <section class="quiz" v-if="!quizCompleted">
        <div class="quiz-info">
          <span class="question">{{ getCurrentQuestion.title }}</span>
          <span class="score"> Puntaje: {{ score }}/{{ questions.length }}</span>
        </div>

        <div class="options">
          <label
              v-for="(option, index) in getCurrentQuestion.options"
              :key="index"
              :class="`option ${
               getCurrentQuestion.selected == index ?
               index === getCurrentQuestion.answer ? 'correct' : 'wrong'
               : ''
              } ${
               getCurrentQuestion.selected != null && index != getCurrentQuestion.selected ?
              'disabled': ''
          }`">
            <input
                type="radio"
                :name="getCurrentQuestion.index"
                v-model="getCurrentQuestion.selected"
                :disabled="getCurrentQuestion.selected"
                @change="setAnswer"
                :value="index">
            <span v-html="option"></span>
          </label>
        </div>

        <button
          @click="nextQuestion"
          :disabled="!getCurrentQuestion.selected">
            {{ getCurrentQuestion.index === questions.length - 1 ? 'Terminar' : 'Siguiente pregunta' }}
        </button>
      </section>

      <div v-if="getCurrentQuestion.selected != null && getCurrentQuestion.answer != getCurrentQuestion.selected && !quizCompleted">
        <div class="meme">
          <img src="https://i.pinimg.com/originals/3d/14/6a/3d146a9b69de8179c118534b83eb1470.jpg" alt="meme_ansiedad"/>
        </div>
      </div>

      <div v-if="getCurrentQuestion.selected != null && getCurrentQuestion.answer == getCurrentQuestion.selected && !quizCompleted">
        <div class="meme">
          <img :src="getCurrentQuestion.correctImage" />
        </div>
      </div>

      <section class="quiz-completed" v-if="quizCompleted">
        <h2>Has terminado el quiz de la actividad de la clase de Diseño y formulación de proyectos!!!</h2>
        <p>Tú puntaje es: {{ score }}/{{ questions.length }}</p>
        <div class="get-better" v-if="score < 3">
          <h2>Tienes que estudiar más, repasa este vídeo para recordar el árbol del problema:</h2>
          <iframe width="560" height="315" src="https://www.youtube.com/embed/eO7sNmZ9Ut8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        </div>

        <div class="get-better" v-else>
          <h2>Excelente, pasaste el quiz. Sigue preparándote con estos temas.</h2>
          <iframe width="560" height="315" src="https://www.youtube.com/embed/eO7sNmZ9Ut8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {

  },
  computed: {
    score () {
      let value = 0;
      this.questions.map(q => {
        if (q.selected == q.answer) value++;
      });
      return value;
    },
    getCurrentQuestion () {
      let question = this.questions[this.currentQuestion];
      question.index = this.currentQuestion;
      console.log(question.index)
      return question;
    }
  },
  data () {
    return {
      currentQuestion: 0,
      quizCompleted: false,
      questions: [
        {
          title: '¿Qué es el arbol del problema?',
          answer: 0,
          options: [
            ' Es una representación gráfica que permite visualizar de manera sistemática un problema central con sus respectivas causas y consecuencias',
            ' Un árbol que causa problemas...',
            ' Un árbol que está compuesto por hojas, raíces, tronco, etc...',
            ' Debería volverme jardinero...'
          ],
          selected: null,
          correctImage: 'https://i.pinimg.com/564x/5d/a6/c4/5da6c489ce787ddbcb9556adbe02fdb9.jpg'
        },
        {
          title: '¿Para que sirve el árbol del problema?',
          answer: 3,
          options: [
            ' Para aprender a dibujar bien un árbol con problemas.',
            ' Sirve pues para causar problemas...',
            ' Sirve para que la profe nos ponga buena nota si lo hacemos bien :)',
            ' Es una herramienta fundamental que nos ayuda poder entender un determinado problema que se debe resolver.'
          ],
          selected: null,
          correctImage: 'https://cdn-5f0f1ae4c1ac181b540e97d1.closte.com/wp-content/uploads/2019/12/meme-contenido-1024x683.jpeg'
        },
        {
          title: '¿Cómo se realiza un árbol de problemas?',
          answer: 1,
          options: [
            ' Se siembra y se riega todos los días.',
            ` PASO 1: Identificar los principales problemas con respecto a la situación en cuestión.<br/>
            PASO 2: Formular en pocas palabras el problema central.<br/>
            PASO 3: Anotar las causas del problema central. <br/>
            PASO 4: Anotar los efectos provocados por el problema central. <br/>
            PASO 5: Elaborar un esquema que muestre las relaciones de un
            Árbol de Problemas.<br/>
            PASO 6: Revisar el esquema completo y verificar su lógica e
            integridad.`,
            ' Se piensa mucho pero mucho, mucho y al final encuentra la solución :)',
            ' Se dibuja el árbol y luego le colocan los problemas.'
          ],
          selected: null,
          correctImage: 'https://media.makeameme.org/created/eso-es-correcto-5bdbc8.jpg'
        },
        {
          title: '¿Qué partes tiene un árbol de problemas?',
          answer: 3,
          options: [
            ' Hoja, Tallo y ramas.',
            ' problema 1, problema 2, problema central, consecuencias.',
            ` En la parte inferior se listan los problemas activos, en la parte central el problema crítico y
           en la parte superior se listan los problemas pasivos.`,
            'Todas son incorrectas.'
          ],
          selected: null,
          correctImage: 'https://pbs.twimg.com/media/DEdshUKWsAAnIrQ.jpg'
        }
      ]
    }
  },
  methods: {
    setAnswer (e) {
      this.questions[this.currentQuestion].selected = e.target.value;
      e.target.value = null;
    },
    nextQuestion () {
      if (this.currentQuestion < this.questions.length - 1) this.currentQuestion++;
      else this.quizCompleted = true;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Lexend Exa', sans-serif;
}

body {
  background-color: #271C36;
  color: #FFFFFF;
}

.quiz, .quiz-completed {
  background: #382a4b;
  padding: 2rem;
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  border-radius: 4px;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #FFFFFF;
  font-size: 1.5rem;
  font-weight: 700;
}

.quiz-info .score {
  color: #2D213F;
  font-size: 1.25rem;
  padding: 0 10px;
  background: #eae515;
  border-radius: 5px;
  box-shadow: rgba(0, 0, 0, 0.07) 0px 1px 2px, rgba(0, 0, 0, 0.07) 0px 2px 4px, rgba(0, 0, 0, 0.07) 0px 4px 8px, rgba(0, 0, 0, 0.07) 0px 8px 16px, rgba(0, 0, 0, 0.07) 0px 16px 32px, rgba(0, 0, 0, 0.07) 0px 32px 64px;
}

.options {
  margin-bottom: 1rem;
}

.options > label {
  font-weight: 500;
}

.option {
  display: block;
  padding: 1rem;
  background-color: #271C36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
  box-shadow: rgba(17, 17, 26, 0.05) 0px 4px 16px, rgba(17, 17, 26, 0.05) 0px 8px 32px;
}

.option:hover {
  background-color: #2D213F;
}

.option.correct {
  background-color: #2cce7d;
  box-shadow: rgba(0, 0, 0, 0.2) 0px 12px 28px 0px, rgba(0, 0, 0, 0.1) 0px 2px 4px 0px, rgba(255, 255, 255, 0.05) 0px 0px 0px 1px inset;
}

.option.wrong {
  background: #ff5a5f;
  box-shadow: rgba(0, 0, 0, 0.2) 0px 12px 28px 0px, rgba(0, 0, 0, 0.1) 0px 2px 4px 0px, rgba(255, 255, 255, 0.05) 0px 0px 0px 1px inset;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

input[type="radio"] {
  display: none;
}

button {
  appearance: none;
  background-color: #2cce7d;
  outline: none;
  border: none;
  padding: 0.5rem 1rem;
  text-transform: uppercase;
  color: #2D213F;
  font-size: 1.25rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

button:disabled {
  opacity: 0.6;
}

.title-app {
  margin: 20px;
  text-align: center;
  font-weight: 900;
}

.meme {
  margin: 20px auto;
  width: 100%;
  max-width: 300px;
  max-height: 100px;
  height: 100%;
}

.meme > img {
  width: 100%;
  height: 100%;
}

.quiz-completed > p {
  text-align: center;
  margin: 20px auto;
  width: 35%;
  color: #2D213F;
  font-size: 1.25rem;
  padding: 10px;
  background: #eae515;
  border-radius: 5px;
  box-shadow: rgba(0, 0, 0, 0.07) 0px 1px 2px, rgba(0, 0, 0, 0.07) 0px 2px 4px, rgba(0, 0, 0, 0.07) 0px 4px 8px, rgba(0, 0, 0, 0.07) 0px 8px 16px, rgba(0, 0, 0, 0.07) 0px 16px 32px, rgba(0, 0, 0, 0.07) 0px 32px 64px;
}

.get-better {
  display: flex;
  justify-content: center;
  flex-direction: column;
}

.get-better > h2 {
  font-weight: 500;
  text-align: center;
}

.get-better > iframe {
  margin: 20px auto;
}

</style>
