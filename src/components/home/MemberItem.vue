<template>
  <tr>
    <th scope="row">{{ index + 1 }}</th>
    <td>{{ member.User.first_name + ' ' + member.User.last_name }}</td>
    <td>{{ member.role }}</td>
    <td>
      <button class="btn btn-primary" @click="changeRole(member.id)">
        <i class="fa fa-edit"></i>
      </button>
      <button
        v-if="member.role === 'member'"
        @click="deleteMember(member.UserId)"
        class="btn btn-danger"
      >
        <i class="fa fa-trash"></i>
      </button>
    </td>
  </tr>
</template>

<script>
export default {
  name: 'MemberItem',
  props: ['member', 'index', 'OrgId'],
  methods: {
    changeRole(id) {},
    deleteMember(member) {
      const id = this.OrgId;
      const data = {
        id,
        member,
      };
      this.$emit('deleteMember', data);
    },
  },
};
</script>

<style></style>
