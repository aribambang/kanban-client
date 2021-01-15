<template>
  <div v-if="boardFormActive === true" class="card">
    <div class="card-body">
      <form @submit.prevent="addBoard">
        <input type="hidden" />
        <div class="form-group">
          <input
            type="text"
            class="form-control"
            v-model="name"
            placeholder="Enter Board Name"
          />
        </div>
        <div class="form-group">
          <button type="submit" class="btn btn-primary w-25">
            Add
          </button>
          <button
            type="button"
            @click="hideBoardForm"
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
  name: 'BoardForm',
  props: ['boardFormActive', 'OrgId'],
  data() {
    return {
      name: '',
    };
  },
  methods: {
    hideBoardForm() {
      this.$emit('hideBoardForm');
    },
    addBoard() {
      const body = {
        name: this.name,
        OrganizationId: this.OrgId,
      };
      this.$emit('addBoard', body);
      this.name = '';
      this.hideBoardForm();
    },
  },
};
</script>

<style></style>
