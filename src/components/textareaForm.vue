<template>
  <div class="form-control">
    <label for="value"> {{ title }} </label>
    <textarea
      id="value"
      rows="3"
			:value="modelValue"
      @input="change"
    ></textarea>
  </div>
</template>

<script>
export default {
	emits: [
		'update:modelValue'
	],
	props: {
		'modelValue': String,
		'title': String
	},
	methods: {
		change() {
			this.$emit('update:modelValue', event.target.value)
		}
	}
};
</script>

<style>
</style>