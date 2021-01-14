<template>
  <tr>
    <th scope="row">{{ index + 1 }}</th>
    <td>{{ org.Organization.name }}</td>
    <td>
      <button class="btn btn-primary" @click="showOrg(org.OrganizationId)">
        <i class="fa fa-eye"></i>
      </button>
      <button
        v-if="org.role === 'admin'"
        @click="deleteOrg(org.OrganizationId)"
        class="btn btn-danger"
      >
        <i class="fa fa-trash"></i>
      </button>
    </td>
  </tr>
</template>

<script>
export default {
  name: 'OrganizationList',
  props: ['index', 'org'],
  methods: {
    deleteOrg(id) {
      this.$emit('deleteOrg', id);
    },
    showOrg(id) {
      this.$emit('showOrg', id);
    },
  },
};
</script>

<style></style>
