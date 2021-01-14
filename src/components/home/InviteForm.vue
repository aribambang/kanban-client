<template>
  <div v-if="inviteFormActive === true" class="card" id="memberForm">
    <div class="card-body">
      <form @submit.prevent="inviteMember">
        <div class="form-group">
          <input
            type="text"
            class="form-control"
            id="mEmail"
            v-model="email"
            placeholder="Enter Email"
          />
        </div>
        <div class="form-group">
          <button type="submit" class="btn btn-primary w-25">
            Add
          </button>
          <button
            type="button"
            @click="hideInviteForm"
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
  name: 'InviteForm',
  props: ['inviteFormActive', 'OrgId'],
  data() {
    return { email: '' };
  },
  methods: {
    hideInviteForm() {
      this.$emit('hideInviteForm');
    },
    inviteMember() {
      const body = { email: this.email };
      const id = this.OrgId;
      this.$emit('inviteMember', { body, id });
      this.email = '';
    },
  },
};
</script>

<style></style>
