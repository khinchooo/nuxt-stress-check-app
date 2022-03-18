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
              <v-card-text>
                {{ categoryByQuestionId(question.id).id }} {{ categoryByQuestionId(question.id).questionText }}
              </v-card-text>
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
            large
            @click="saveBtn"
          >
           完了
          </v-btn>
        </v-card-actions>
      </div>
    </v-card>
  </v-container>
</template>

<script>
import { collection, addDoc, serverTimestamp } from '@firebase/firestore'
import { db } from '../plugins/firebase'
const resultsCollectionRef = collection(db, 'results')

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
      nextBtn: true,
      totalA: 0,
      totalB: 0,
      totalC: 0,
      totalD: 0,
      total: 0,
      hightStress: '',
      totalAC: 0
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
      const categoryId = this.categoryIdByQuestionId(questionId)
      return this.categories.find(category => category.id === categoryId)
    },
    next () {
      if (!this.checked) {
        alert("答えを選んでください。")
        return
      }
      // do answer
      this.doAnswer()
      // show board
      this.boarding = this.boarding + 1 === this.questions.length ? 0 : this.boarding + 1
      // show answer
      this.showSelectedAnswer()
      // button disabled or enabled
      this.btnDisabledEnabled()
    },
    prev () {
      // show board
      this.boarding = this.boarding - 1 < 0 ? this.questions.length - 1 : this.boarding - 1
      // show answer
      this.showSelectedAnswer()
      // button disabled or enabled
      this.btnDisabledEnabled()
    },
    btnDisabledEnabled() {
      this.prevBtn = this.boarding > 0
      this.nextBtn = this.boarding < this.questions.length - 1
    },
    showSelectedAnswer() {
      const answer = this.answers.find(answer => answer.questionId === this.questionId)
      if (answer != null) {
        this.answerPoint = answer.answerPoint
      } else {
        this.answerPoint = null
      }
    },
    doAnswer() {
      const qusId = this.questionId
      const answer = {
        categoryId: this.categoryIdByQuestionId(qusId),
        questionId: qusId,
        answerPoint: this.answerPoint
      }
      const answerIndex = this.answers.findIndex(answer => answer.questionId === qusId)
      if (answerIndex >= 0) {
        this.answers[answerIndex] = answer
      } else {
        this.answers.push(answer)
      }
    },
    saveBtn () {
      this.answers.forEach(answer => {
        if (answer.categoryId === 'A') {
          this.totalA += Number(answer.answerPoint)
        } else if (answer.categoryId === 'B') {
          this.totalB += Number(answer.answerPoint)
        } else if (answer.categoryId === 'C') {
          this.totalC += Number(answer.answerPoint)
        } else if (answer.categoryId === 'D') {
          this.totalD += Number(answer.answerPoint)
        } else if (answer.categoryId === 'A' || answer.categoryId === 'C' ) {
          this.totalAC += Number(answer.answerPoint)
        }
      });

      // let highStressFlag = false
      if (this.totalB >= 77 || (this.totalB >= 63 && this.totalAC >= 76)) {
        this.hightStress = '高ストレス'
      } else {
        this.hightStress = '異常なし'
      }
      // add results
      addDoc(resultsCollectionRef, {
        user_id: this.user.uid,
        user_name: this.user.displayName,
        totalA: this.totalA,
        totalB: this.totalB,
        totalC: this.totalC,
        totalD: this.totalD,
        total: this.answers.reduce((p, c) => p + Number(c.answerPoint), 0),
        hightStress: this.hightStress,
        checkDate: serverTimestamp()
      }).then(() => {
        this.$router.push({ name: 'index' })
        this.answers = []
      })
    },
    categoryIdByQuestionId(questionId) {
      return questionId.substring(0, 1)
    }
  }
}
</script>
