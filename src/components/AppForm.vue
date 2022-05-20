<template>
  <form class="card card-w30" @submit.prevent="addContent">

    <div class="form-control">
      <label for="type"> {{ label1 }} </label>
      <select id="type" v-model="typeOfBlock">
        <option v-for="(type, idx) in types" :key="idx" :value="type.value">
          {{ type.label }}
        </option>
      </select>
    </div>

    <div class="form-control">
      <label for="value"> {{ label2 }} </label>
      <textarea
        id="value"
        rows="3"
        @keydown.enter="addContent"
        v-model.trim="textarea"
      ></textarea>
    </div>

    <button class="btn primary" :disabled="isDisabled"> {{ buttonText }} </button>
  </form>
</template>

<script>
export default {
	props: {
		'addContent': Function,
		'label1': String,
		'label2': String,
		'typeOfBlock': String,
		'types': Array,
		'textarea': String,
		'isDisabled': Function,
		'buttonText': String
	}
};
</script>

<style>
</style>