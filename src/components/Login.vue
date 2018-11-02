<template>
  <div>
    <div class="container" v-if="!logged">
      <h1>Admin login</h1>
      <div class="content">
        <transition>
          <div class="error" v-if="errMessage">
            <p>
              Invalid email or password.
            </p>
          </div>
        </transition>
        <form>
          <div class="form-control">
            <label>Email</label>
            <input type="text" v-model="email">
          </div>
          <div class="form-control">
            <label>Password</label>
            <input type="text" v-model="password">
          </div>
          <button @click="login">Login</button>
        </form>
      </div>
    </div>
    <Dashboard v-if="logged" />
    <div class="container" v-if="logged">
      <button @click="logout">Logout</button>
    </div>
  </div>
</template>

<script>
import Dashboard from './Dashboard'
import firebase from 'firebase'

export default {
  components: {
    Dashboard
  },
  data () {
    return {
      logged: false,
      errMessage: false,
      email: '',
      password: ''
    }
  },
  methods: {
    login: function (e) {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then(() => {
            this.logged = true,
            this.errMessage = false
          }
        ).catch(() => {
          this.errMessage = true
        })
        // .then(
        //   user => {
        //     this.logged = true,
        //     this.errMessage = false
        //   },
        //   err => {
        //     this.errMessage = true
        //   }
        // )
      e.preventDefault()
    },
    logout: function () {
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.logged = false
        })
    }
  }
}
</script>

<style lang="sass" scoped>
.content
  .error
    background: #c64343
    color: white
    padding: 40px 20px
    margin: 20px
    border-radius: 8px
    font-size: 22px
    text-align: center
  form
    width: 80%
    margin: 20px auto
    background: #57b633
    padding: 20px
    border-radius: 8px
    transition: .5s
    .form-control
      label, input
        width: 100%
        height: 30px
        margin-bottom: 10px
      label
        color: white
      input
        padding-left: 10px

button
  display: block
  margin: 10px auto
  padding: 10px 30px
  border: none
  border-radius: 5px
  background: #fb6e08
  color: white
  font-size: 20px
  cursor: pointer

.v-enter-active, .v-leave-active
  transition: all .4s
.v-enter, .v-leave-to
  opacity: 0
  transform: scale(0)

</style>
