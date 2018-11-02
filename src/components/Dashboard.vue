<template>
  <div>
    <div class="container">
      <h1>Admin Dashboard</h1>
      <div class="messages">
        <ul>
          <li>
            <h4>Messages</h4>
          </li>
          <li v-for="(message, index) in messages" :key="index">
            <div class="cell name">
              {{ message.name }}
            </div>
            <div class="cell email">
              {{ message.email }}
            </div>
            <div class="cell message">
              {{ message.message }}
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit'

export default {
  data () {
    return {
      messages: []
    }
  },
  created () {
    db.collection('Messages').get().then(querySnapshot => {
      querySnapshot.forEach(doc => {
        const data = {
          'id': doc.id,
          'name': doc.data().name,
          'email': doc.data().email,
          'message': doc.data().message
        }
        this.messages.push(data)
      })
    })
  }
}
</script>

<style lang="sass" scoped>
  .messages
    margin-top: 60px
    li
      width: 100%
      padding: 10px
      border: 1px solid #bbb
      h4
      .cell
        .name
          width: 20%
        .email
          width: 20%
        .message
          width: 60%


</style>
