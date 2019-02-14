<template>
  <div>
    <div class="projects">
      <h4>Projects</h4>
      <ul v-for="project in sortProjects(projects)" :key="project.id">
        <li>
          <div class="box">
            {{ project.id }}
          </div>
          <div class="box">
            {{ project.name }}
          </div>
          <div class="box">
            {{ project.category }}
          </div>
        </li>
      </ul>
    </div>
    <h1>Add Project</h1>
    <form @submit.prevent="AddProject">
      <div class="success" v-if="success">
        <p>Project added</p>
      </div>
      <div class="fail" v-if="fail">
        <p>Adding project failed</p>
      </div>
      <div class="form-control">
        <label>Project Name</label>
        <input v-model="name" @click="resetMessages">
      </div>
      <div class="form-control">
        <label>Project Id</label>
        <input v-model="id">
      </div>
      <div class="form-control">
        <label>Project Info</label>
        <textarea v-model="info"></textarea>
      </div>
      <div class="form-control">
        <label>Project Link</label>
        <input v-model="link">
      </div>
      <div class="form-control">
        <label>Github Link</label>
        <input v-model="github">
      </div>
      <div class="form-control">
        <label>Image</label>
        <input v-model="image">
      </div>
      <div class="form-control">
        <!-- <fieldset> -->
        <legend>Select Tags</legend>
        <ul>
          <li v-for="(tag, index) in tags" :key="index">
            <input type="checkbox" v-model="item.tag" :id="'tag_' + tag.ig" :value="tag.name">
            <label>{{ tag.name }}</label>
          </li>
        </ul>
        <!-- </fieldset> -->
      </div>
      <div class="form-control">
        <label>Select Category</label>
        <select v-model="category">
          <option value="resp">Responsive Design</option>
          <option value="js">Java Script</option>
          <option value="php">PHP</option>
        </select>
      </div>
      <button type="submit">Add Project</button>
    </form>
  </div>
</template>

<script>
import db from './firebaseInit'

export default {
  data () {
    return {
      projects: [],
      success: false,
      fail: false,
      name: '',
      id: '',
      info: '',
      link: '',
      github: '',
      image: '',
      tags: [
        {
          'id': 1,
          'name': 'HTML'
        },
        {
          'id': 2,
          'name': 'CSS'
        },
        {
          'id': 3,
          'name': 'SCSS'
        },
        {
          'id': 4,
          'name': 'SASS'
        },
        {
          'id': 5,
          'name': 'Bootstrap'
        },
        {
          'id': 6,
          'name': 'Materialize'
        },
        {
          'id': 7,
          'name': 'JavaScript'
        },
        {
          'id': 8,
          'name': 'jQuery'
        },
        {
          'id': 9,
          'name': 'PHP'
        },
        {
          'id': 10,
          'name': 'Laravel'
        },
        {
          'id': 11,
          'name': 'Vue JS'
        },
        {
          'id': 12,
          'name': 'Firebase'
        },
        {
          'id': 13,
          'name': 'Axios'
        },
        {
          'id': 14,
          'name': 'API'
        }
      ],
      item: {
        tag: []
      },
      category: ''
    }
  },
  created () {
    db.collection('Project').get().then(snapshot => {
      snapshot.forEach(doc => {
        const data = {
          'name': doc.data().name,
          'id': doc.data().id,
          'link': doc.data().link,
          'category': doc.data().category
        }
        this.projects.push(data)
      })
    })
  },
  methods: {
    AddProject () {
      db.collection('Project').add({
        name: this.name,
        id: this.id,
        info: this.info,
        link: this.link,
        github: this.github,
        image: this.image,
        tags: this.tags,
        item: this.item,
        showTags: false,
        category: this.category
      }).then(() => {
        this.success = true
        this.fail = false
      }).catch(() => {
        this.fail = true
        this.success = false
      })
    },
    resetMessages () {
      this.fail = false
      this.success = false
      this.name = ''
      this.info = ''
      this.link = ''
      this.github = ''
      this.image = ''
      this.category = ''
    },
    sortProjects: function (el) {
      return el.slice().sort(function (a, b) {
        return a.id - b.id
      })
    }
  }
}
</script>

<style lang="sass" scoped>
.projects
  margin: 30px auto
  border: 1px solid #999
  border-radius: 5px
  background: #f5f5f5
  padding: 10px
  h4
    border-bottom: 1px solid #999
    margin-bottom: 5px
    padding-bottom: 5px
  ul
    li
      display: flex
      border-bottom: 1px solid #999
      padding-bottom: 5px
      margin-bottom: 5px
      .box
        width: 100%
        margin-right: 20px
        &:first-child
          width: 60px
        &:last-child
          text-align: right
          margin: 0
form
  width: 100%
  margin: 20px auto
  padding: 40px
  border: 1px solid #bbb
  background: #efefef
  border-radius: 5px
  .success
    p
      padding: 20px
      font-size: 22px
      color: green
  .fail
    p
      padding: 20px
      font-size: 22px
      color: red
  .form-control
    label, input, textarea
      width: 100%
      margin-bottom: 7px
    label
      display: block
      margin-top: 10px
    input
      height: 35px
      border: 1px solid #bbb
      border-radius: 4px
      padding-left: 10px
    legend
    ul
      display: flex
      flex-direction: row
      flex-wrap: wrap
      margin: 10px 0
      li
        display: flex
        input
          width: auto
          height: auto
          margin: 0
        label
          width: auto
          margin: 0
          margin-left: 5px
          margin-right: 20px
    textarea
      resize: none
      height: 90px
    select
      height: 35px
      width: 200px
      font-size: 20px
      border: 1px solid #bbb
      border-radius: 4px
      option
  button
    padding: 15px 30px
    margin: 20px auto
    display: flex
    border: none
    border-radius: 5px
    font-size: 20px
    background: #18b128
    color: white
    cursor: pointer
    transition: .4s
    &:hover
      background: #2c99f2

@media only screen and (max-width: 500px)
  form
    padding: 20px
</style>
