<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addContent">
      <!-- <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="type">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div> -->
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="type">
          <option
            v-for="(type, idx) in types"
            :key="idx"
            :value="type.value"
          >
            {{type.label}}
          </option>    
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" @keydown.enter="addContent" v-model.trim="contentText"></textarea>
      </div>

      <button class="btn primary" :disabled="isDisabled">Добавить</button>
    </form>

    <app-resume-block 
      :typeOfBlock="type"
      :content="contentBlocks"
      :textarea="contentText"
    ></app-resume-block> 
    <!-- resumeBlock -->
    
  </div>
  <!-- comments -->
  <app-comments
    @addComments="loadComments"
    :isOpen="isOpenComments"
    :isNotLoad="loading"
    :comments="comments"
  ></app-comments>
  <!-- <app-loading v-if="loading"></app-loading> -->
</template>

<script>
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
      loading: false
    }
  },
  mounted() {
    this.loadContent()
  },
  methods: {
    async addContent () {    
      const response = await fetch('https://course-work2-67ded-default-rtdb.firebaseio.com/resume.json', {
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
      const response =  await fetch('https://course-work2-67ded-default-rtdb.firebaseio.com/resume.json')
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
    // AppLoading
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
