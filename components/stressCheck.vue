<template>
  <v-container class="text-center">
    <v-card>
      <div class="pt-5">
        <div class="text-right">
          {{ boarding + 1 }} / {{ questions.length }}
        </div>
        <v-window v-model="boarding" class="slide-window">
          <v-window-item v-for="(question, i) in questions" :key="`card-${i}`">
            <v-card
              color="transparent"
            >
              <v-row
                class="fill-height text-center"
                align="center"
                justify="center"
              >
                <v-col cols="12">
                  <v-card-text>
                    {{ categoryByQuestionId(question.id).id }} {{ categoryByQuestionId(question.id).questionText }}
                  </v-card-text>
                </v-col>
                <v-col cols="12">
                  <v-card-text>
                    {{ question.id }} {{ question.answerText }}
                  </v-card-text>
                  <v-radio-group v-model="answerPoint">
                    <v-radio
                      v-for="(answerPointIdx, j) in question.answerPoint"
                      :key="j"
                      :label="`${answerText(question.id, j)}`"
                      :value="answerPointIdx"
                    ></v-radio>
                  </v-radio-group>
                </v-col>
              </v-row>
            </v-card>
          </v-window-item>
        </v-window>
        <v-card-actions class="justify-space-between">
          <v-btn
            v-if="prevBtn"
            btn
            icon
            large
            @click="prev"
          >
            <v-icon>mdi-chevron-left</v-icon>
          </v-btn>
          <v-btn
            v-else
            btn
            icon
            large
            disabled
          >
            <v-icon>mdi-chevron-left</v-icon>
          </v-btn>
          <v-btn
            v-if="nextBtn"
            btn
            icon
            large
            @click="next"
          >
            <v-icon>mdi-chevron-right</v-icon>
          </v-btn>
          <v-btn
            v-else
            btn
            icon
            large
            disabled
          >
            <v-icon>mdi-chevron-right</v-icon>
          </v-btn>
        </v-card-actions>
      </div>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'StressCheckerPage',
  props: {
    categories: {
      type: Array,
      default: null
    },
    questions: {
      type: Array,
      default: null
    }
  },
  data () {
    return {
      boarding: 0,
      answerPoint: null,
      answers: [],
      prevBtn: false,
      nextBtn: true
    }
  },
  computed: {
    user () {
      return this.$store.state.user
    },
    checked () {
      return this.answerPoint != null && this.answerPoint > 0
    },
    questionId () {
      return this.questions[this.boarding].id
    },
  },
  methods: {
    answerText(questionId, idx) {
      return this.categoryByQuestionId(questionId).answerText[idx]
    },
    categoryByQuestionId(questionId) {
      const categoryId = questionId.substring(0, 1)
      return this.categories.find(category => category.id === categoryId)
    },
    next () {
      if (!this.checked) {
        alert("choose")
        return
      }
      // do answer
      this.doAnswer()
      // show board
      this.boarding = this.boarding + 1 === this.questions.length ? 0 : this.boarding + 1
      // show answer
      this.showAnswer()
      // button disabled or enabled
      this.btnDisabledEnabled()
    },
    prev () {
      // show board
      this.boarding = this.boarding - 1 < 0 ? this.questions.length - 1 : this.boarding - 1
      // show answer
      this.showAnswer()
      // button disabled or enabled
      this.btnDisabledEnabled()
    },
    btnDisabledEnabled() {
      this.prevBtn = this.boarding > 0
      this.nextBtn = this.boarding < this.questions.length - 1
    },
    showAnswer() {
      const answer = this.answers.find(answer => answer.questionId === this.questionId)
      if (answer != null) {
        this.answerPoint = answer.answerPoint
      } else {
        this.answerPoint = null
      }
    },
    doAnswer() {
      const answer = {
        questionId: this.questionId,
        answerPoint: this.answerPoint
      }
      const answerIndex = this.answers.findIndex(answer => answer.questionId === this.questionId)
      if (answerIndex >= 0) {
        this.answers[answerIndex] = answer
      } else {
        this.answers.push(answer)
      }
    }
  }
}
</script>
