<template>
  <div v-if="OrgId !== null" class="col-8" id="projectContent">
    <h3>Organization</h3>
    <div class="mb-3">
      <button class="btn btn-primary w-25" @click="showInviteForm">
        Invite Member
      </button>
      <button class="btn btn-primary w-25" @click="showMemberList">
        List Members
      </button>
    </div>
    <invite-form
      :inviteFormActive="inviteFormActive"
      :OrgId="OrgId"
      @hideInviteForm="hideInviteForm"
      @inviteMember="inviteMember"
    ></invite-form>
    <member-list
      :OrgId="OrgId"
      :membersList="membersList"
      :membersListActive="membersListActive"
      @deleteMember="deleteMember"
    ></member-list>
    <h3>Boards:</h3>
    <div class="mb-3">
      <button class="btn btn-primary w-25" @click="showBoardForm">
        Add Boards
      </button>
    </div>
    <board-form
      :boardFormActive="boardFormActive"
      @hideBoardForm="hideBoardForm"
    ></board-form>
    <hr />
    <div id="projectTodoList"></div>
  </div>
</template>

<script>
import BoardForm from './BoardForm';
import InviteForm from './InviteForm';
import MemberList from './MemberList';

export default {
  components: { InviteForm, BoardForm, MemberList },
  name: 'OrganizationContent',
  props: ['OrgId', 'membersList'],
  data() {
    return {
      inviteFormActive: false,
      boardFormActive: false,
      membersListActive: false,
    };
  },
  methods: {
    showInviteForm() {
      this.inviteFormActive
        ? (this.inviteFormActive = false)
        : (this.inviteFormActive = true);

      this.boardFormActive = false;
    },
    hideInviteForm() {
      this.inviteFormActive = false;
    },
    showBoardForm() {
      this.boardFormActive
        ? (this.boardFormActive = false)
        : (this.boardFormActive = true);
      this.inviteFormActive = false;
    },
    hideBoardForm() {
      this.boardFormActive = false;
    },
    showMemberList() {
      this.membersListActive
        ? (this.membersListActive = false)
        : (this.membersListActive = true);
    },

    inviteMember(v) {
      this.$emit('inviteMember', v);
    },
    deleteMember(v) {
      this.$emit('deleteMember', v);
    },
  },
};
</script>

<style></style>
