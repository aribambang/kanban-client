<template>
  <tr>
    <th scope="row">{{ index + 1 }}</th>
    <td>{{ org.Organization.name }}</td>
    <td>
      <button class="btn btn-primary" onclick="showProjectContent(${data.id})">
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
import axios from 'axios';
export default {
  name: 'OrganizationList',
  props: ['index', 'org'],
  methods: {
    deleteOrg(id) {
      this.$emit('deleteOrg', id);
    },
  },
};
</script>

<style></style>
