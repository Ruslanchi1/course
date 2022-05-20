<template>
  <div class="container column">

    <app-form
      :addContent="addContent"
      label1="Тип заголовка"
      label2="Значение"
      :typeOfBlock="type"
      :types="types"
      :textarea="contentText"
      :isDisabled="isDisabled"
      buttonText="Добавить"
    ></app-form>

    <app-resume-block 
      :typeOfBlock="type"
      :content="contentBlocks"
      :textarea="contentText"
    ></app-resume-block> 
    
  </div>

  <app-comments
    @addComments="loadComments"
    :isOpen="isOpenComments"
    :isNotLoad="loading"
    :comments="comments"
    buttonText="Загрузить комментарии"
    title="Комментарии"
  ></app-comments>
  
</template>

<script>
import AppForm from './components/AppForm'
import AppResumeBlock from './components/AppResumeBlock'
import AppComments from './components/AppComments'
// import AppLoading from './components/AppLoader.vue'

export default {
  data () {
    return {
      type: 'title',
      types: [
        {
          label: 'Заголовок',
          value: 'title'
        },
        {
          label: 'Подзаголовок',
          value: 'subtitle'
        },
        {
          label: 'Аватар',
          value: 'avatar'
        },
        {
          label: 'Текст',
          value: 'text'
        },
      ],
      contentBlocks: [],
      comments: [],
      contentText: '',
      isOpenComments: false,
      loading: false,
      resumeResponse: 'https://course-work2-67ded-default-rtdb.firebaseio.com/resume.json'
    }
  },
  mounted() {
    this.loadContent()
  },
  methods: {
    async addContent () {    
      const response = await fetch( this.resumeResponse, {
        method: 'POST',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify({
          text: this.contentText,
          type: this.type
        })
      })

      const firebaseData = await response.json()

      this.contentBlocks.push({
        id: firebaseData.name,
        text: this.contentText,
        type: this.type
      })
      
      this.contentText = ''
    },
    async loadContent () {
      const response =  await fetch( this.resumeResponse)
      const data = await response.json()

      this.contentBlocks = Object.keys(data).map( key => {
        return {
          id: key,
          ...data[key]
        }
      })
    },
    async loadComments () {
      this.loading = true

      const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42')
      const data = await response.json()

      this.comments = Object.keys(data).map( key => {
        return {
          ...data[key]
        }
      })

      this.isOpenComments = true
      this.loading = false
    }
  },
  computed: {
    isDisabled () {
      return this.contentText.length < 4
    }
  },
  components: {
    AppResumeBlock,
    AppComments,
    AppForm
  }
}
</script>

<style>

</style>
