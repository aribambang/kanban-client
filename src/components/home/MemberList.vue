<template>
  <table v-if="membersListActive === true" class="table table-striped mt-3">
    <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">Name</th>
        <th scope="col">Role</th>
        <th scope="col">Action</th>
      </tr>
    </thead>
    <tbody>
      <member-item
        v-for="(member, index) in membersList"
        :key="member.id"
        :member="member"
        :index="index"
        :OrgId="OrgId"
        @deleteMember="deleteMember"
      ></member-item>
    </tbody>
  </table>
</template>

<script>
import MemberItem from './MemberItem';
export default {
  name: 'MemberList',
  components: { MemberItem },
  props: ['membersListActive', 'membersList', 'OrgId'],
  methods: {
    deleteMember(v) {
      this.$emit('deleteMember', v);
    },
  },
};
</script>

<style></style>
