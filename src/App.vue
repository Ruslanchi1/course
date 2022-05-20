<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addContent">

      <select-form
        v-model="type"
        title="Тип заголовка"
        :types="types"
      ></select-form>

      <textarea-form
        v-model.trim="contentText"
        title="Значение"
		  	@keydown.enter="addContent"
      ></textarea-form>

      <app-button
        :text="contentText"
        buttonText="Добавить"
      ></app-button>
    </form>

    <app-resume-block
      :typeOfBlock="type"
      :content="contentBlocks"
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
// import AppForm from "./components/AppForm";
import AppResumeBlock from "./components/AppResumeBlock";
import AppComments from "./components/AppComments";
import AppButton from "./components/AppButton.vue"
import selectForm from "./components/selectForm.vue"
import textareaForm from "./components/textareaForm.vue";

export default {
  data() {
    return {
      type: "title",
      types: [
        {
          label: "Заголовок",
          value: "title",
        },
        {
          label: "Подзаголовок",
          value: "subtitle",
        },
        {
          label: "Аватар",
          value: "avatar",
        },
        {
          label: "Текст",
          value: "text",
        },
      ],
      contentBlocks: [],
      comments: [],
      contentText: '',
      isOpenComments: false,
      loading: false,
      resumeResponse:
        "https://course-work2-67ded-default-rtdb.firebaseio.com/resume.json",
    };
  },
  mounted () {
    this.loadContent()
  },
  methods: {
    async addContent() {
      const response = await fetch(this.resumeResponse, {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify({
          text: this.contentText,
          type: this.type,
        }),
      });

      const firebaseData = await response.json();

      this.contentBlocks.push({
        id: firebaseData.name,
        text: this.contentText,
        type: this.type,
      });

      this.contentText = '';
    },
    async loadContent() {
      const response = await fetch(this.resumeResponse);
      const data = await response.json();

      this.contentBlocks = Object.keys(data).map( (key) => {
        return {
          id: key,
          ...data[key],
        };
      });
      console.log('dsfasd')
    },
    async loadComments() {
      this.loading = true;

      const response = await fetch(
        "https://jsonplaceholder.typicode.com/comments?_limit=42"
      );
      const data = await response.json();

      this.comments = Object.keys(data).map((key) => {
        return {
          ...data[key],
        };
      });

      this.isOpenComments = true;
      this.loading = false;
    },
  },

  components: {
    AppResumeBlock,
    AppComments,
    AppButton,
    textareaForm,
    selectForm
  },
};
</script>

<style>
</style>
