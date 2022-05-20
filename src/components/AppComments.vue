<template>
  <div class="container">
    <p v-if="!isOpen">
      <button class="btn primary" @click="$emit('addComments')"> {{ buttonText }} </button>
    </p>
    <div class="card" v-if="isOpen">
      <h2> {{ title }} </h2>
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

    <app-loader v-if="isNotLoad"></app-loader>
    
  </div>
</template>

<script>
import AppLoader from './AppLoader.vue'

export default {
  emits: ['addComments'],
  props:{ 
    'isOpen': Boolean,
    'isNotLoad': Boolean,
    'comments': Array,
    'buttonText': String,
    'title': String
  },
  components: {
    AppLoader
  }
};
</script>

<style>
</style>