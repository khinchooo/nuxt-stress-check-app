<template>
  <div>
    <v-row>
      <v-col>
        <base-form />
        <stress-check :categories="categories" :questions="questions" />
      </v-col>
    </v-row>
  </div>
</template>

<script>
import { collection, onSnapshot } from '@firebase/firestore'
import { db } from '../plugins/firebase'
const categorieCollectionRef = collection(db, 'categories')
const questionCollectionRef = collection(db, 'questions')

export default {
  name: 'StartPage',
  data () {
    return {
      categories: [],
      questions: []
    }
  },
  computed: {
    isAuthenticated () {
      return this.$store.getters.isAuthenticated
    }
  },
  created () {
    if (!this.isAuthenticated) {
      this.$router.push('/login')
    }
  },
  mounted () {
    onSnapshot(categorieCollectionRef, (querySnapshot) => {
      this.categories = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
    })
    onSnapshot(questionCollectionRef, (querySnapshot) => {
      this.questions = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
    })
  }
}
</script>

<style>

</style>
