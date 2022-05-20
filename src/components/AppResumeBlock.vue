<template>
  <div class="card card-w70">

    <div v-if="content.length !== 0">
	    <component
				v-for="block in content"
				:key="block.id"
        :is=" 'form-' + block.type"
				:text="block.text"
			>{{ block.text }}</component>
    </div>

    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>

  </div>
</template>

<script>
import formTitle from "./formTitle.vue";
import formSubtitle from "./formSubtitle.vue";
import formAvatar from "./formAvatar.vue";
import formText from "./formText.vue";

export default {
  props: {
		'typeOfBlock': String,
		'content': Array
	},
  components: {
    formTitle,
    formSubtitle,
    formAvatar,
    formText,
  },
};
</script>