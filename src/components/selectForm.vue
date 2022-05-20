<template>
  <div class="form-control">
    <label for="type"> {{ title }} </label>
    <select id="type" @change="change">
      <option v-for="(type, idx) in types" :key="idx" :value="type.value">
        {{ type.label }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
	emits: [
		'update:modelValue'
	],
	props: {
		'types': Array,
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