<template>
  <div class="card card-w70">

    <div v-if="content.length !== 0">
	    <component
				v-for="block in content"
				:key="block.id" :is=" 'app-' + block.type"
				:text="block.text"
			>{{ block.text }}</component>
    </div>

    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>

  </div>
</template>

<script>
import AppTitle from "./AppTitle.vue";
import AppSubtitle from "./AppSubtitle.vue";
import AppAvatar from "./AppAvatar.vue";
import AppText from "./AppText.vue";

export default {
  props: {
		'typeOfBlock': String,
		'content': Array
	},
  components: {
    AppTitle,
    AppSubtitle,
    AppAvatar,
    AppText,
  },
};
</script>