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
            btn
            icon
            large
            class="text-decoration-none"
            @click="prev"
          >
            <v-icon>mdi-chevron-left</v-icon>
          </v-btn>
          <v-btn
            btn
            icon
            large
            @click="next"
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
      answerPoint: null
    }
  },
  computed: {
    user () {
      return this.$store.state.user
    },
    checked () {
      return this.answerPoint != null && parseInt(this.answerPoint) > 0
    }
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
      this.answerPoint = null
      this.boarding = this.boarding + 1 === this.questions.length ? 0 : this.boarding + 1
    },
    prev () {
      this.boarding = this.boarding - 1 < 0 ? this.questions.length - 1 : this.boarding - 1
    },
  }
}
</script>
