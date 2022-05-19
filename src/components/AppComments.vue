<template>
  <div class="container">
    <p v-if="!isOpen">
      <button class="btn primary" @click="$emit('addComments')">Загрузить комментарии</button>
    </p>
    <div class="card" v-if="isOpen">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="comment in comments" :key="comment.id">
          <div>
            <p><strong>{{ comment.email }}</strong></p>
            <small
              > {{ comment.body}} </small
            >
          </div>
        </li>
      </ul>
    </div>
    <!-- <div v-else class="loader"></div> -->
    <app-loader v-if="isNotLoad"></app-loader>
  </div>
</template>

<script>
import AppLoader from './AppLoader.vue'

export default {
  emits: ['addComments'],
  props: [
    'isOpen',
    'isNotLoad',
    'comments'
  ],
  components: {
    AppLoader
  }
};
</script>

<style>
</style>