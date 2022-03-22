<template>
<div>
  <base-form />
  <result-form :result="result" />
</div>
</template>

<script>
import { collection, onSnapshot, query, orderBy } from 'firebase/firestore'
import { db } from '../plugins/firebase'

export default {
  name: 'EndPage',
  data () {
    return {
      results: []
    }
  },
  computed: {
    user () {
      return this.$store.state.user
    },
    result () {
      return this.results.length ? this.results[0] : {}
    }
  },
  mounted () {
    const resultsCollectionRef = collection(db, 'results')
    const q = query(resultsCollectionRef, orderBy('checkDate', 'desc'))
    onSnapshot(q, (querySnapshot) => {
      this.results = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
    })
  }
}
</script>
