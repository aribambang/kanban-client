<template>
  <div v-if="orgFormActive === true" class="card mt-2" id="orgForm">
    <div class="card-body">
      <form @submit.prevent="addOrg">
        <div class="form-group">
          <input
            type="text"
            class="form-control"
            id="nameOrganization"
            v-model="name"
            placeholder="Enter Name Organization"
          />
        </div>
        <div class="form-group">
          <button type="submit" class="btn btn-primary w-25">
            Create
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
  props: ['orgFormActive', 'host'],
  data() {
    return {
      name: '',
    };
  },
  methods: {
    hideOrgForm() {
      this.$emit('hideOrgForm');
    },
    addOrg() {
      const body = {
        name: this.name,
      };
      this.$emit('addOrg', body);
      this.name = '';
    },
  },
};
</script>

<style></style>
