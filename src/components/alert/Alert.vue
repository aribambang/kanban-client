<template>
  <div
    :class="[
      'alert alert-dismissible fade show',
      type === 'error'
        ? 'alert-danger'
        : type === 'success'
        ? 'alert-success'
        : '',
    ]"
    role="alert"
    v-if="activeAlert === true"
  >
    <strong>{{ type }}!</strong> {{ message }}
    <button
      type="button"
      class="close"
      @click="hide"
      data-dismiss="alert"
      aria-label="Close"
    >
      <span aria-hidden="true">&times;</span>
    </button>
  </div>
</template>

<script>
export default {
  name: 'Alert',
  props: ['type', 'message', 'activeAlert'],
  methods: {
    hide() {
      this.$emit('hideAlert');
    },
  },
};
</script>

<style></style>
