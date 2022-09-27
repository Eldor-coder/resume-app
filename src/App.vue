<template>
  <div class="container column">
    <resume-form 
      @add-block="addedBlock"  
    ></resume-form>
    <resume-view
       :blocks="blocks"
    ></resume-view>
  </div>
  <div class="container">
      <app-loader v-if="loading"></app-loader>
      <resume-comments 
         v-else
         @comment-btn="showComments"
         :authors="authors"
      ></resume-comments>
  </div>
</template>

<script>
import axios from 'axios'
import ResumeForm from './components/ResumeForm.vue'
import ResumeView from './components/ResumeView.vue'
import ResumeComments from './components/ResumeComments.vue'
import AppLoader from './components/AppLoader.vue'

export default {
  components : {
    ResumeForm,
    ResumeView,
    ResumeComments,
    AppLoader
  },
  data() {
    return {
      blocks : [],
      authors : [],
      loading : false,
      url : 'https://vue-resume-app-f1804-default-rtdb.firebaseio.com/resume.json',
    }
  },
  methods: {
    async addedBlock(val){
      this.blocks.push(val)
      const response = await fetch('https://vue-resume-app-f1804-default-rtdb.firebaseio.com/resume.json',{
        method : 'POST',
        headers : {
          'Content-type' : 'application/json'
        },
        body : JSON.stringify({
          blocks : val
        })
      })
      const firebaseData = await response.json()
      console.log(firebaseData);
    },
    showComments(){
      this.loading = true
      axios.get('https://jsonplaceholder.typicode.com/users')
      .then(res => {
        this.authors = res.data
      })
      if(this.authors == []){
        this.loading = true
      } else {
         setTimeout(() => {
          this.loading = false
         },1500)
      }
     
    },
    getBlocks(){
      axios.get('https://vue-resume-app-f1804-default-rtdb.firebaseio.com/resume.json')
      .then(res => {
        this.blocks = Object.keys(res.data).map(key => {
          console.log(key, res.data[key])
          return {
            ...res.data[key]
          }
          })
        console.log(res.data);
        console.log(this.blocks);
      })
    }
  },
  mounted(){
    this.getBlocks()
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
