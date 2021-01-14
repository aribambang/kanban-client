<template>
  <div v-if="orgFormActive === true" class="card mt-2" id="orgForm">
    <div class="card-body">
      <form onsubmit="addProject(event)">
        <div class="form-group">
          <input
            type="text"
            class="form-control"
            id="nameProject"
            placeholder="Enter Name Organization"
          />
        </div>
        <div class="form-group">
          <button type="submit" class="btn btn-primary w-25">
            Add
          </button>
          <button
            type="button"
            @click="hideOrgForm"
            class="btn btn-danger w-25"
          >
            Close
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OrganizationForm',
  props: ['orgFormActive'],
  methods: {
    hideOrgForm() {
      this.$emit('hideOrgForm');
    },
  },
};
</script>

<style></style>
