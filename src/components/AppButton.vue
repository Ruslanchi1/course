<template>
	<button class="btn primary" :disabled="isDisabled"> {{ buttonText }} </button>
</template>

<script>
export default {
	props: {
		'buttonText': String,
		'text': String
	},
  computed: {
    isDisabled () {
      return this.text.length < 4
    }
  },
}
</script>

<style>

</style>