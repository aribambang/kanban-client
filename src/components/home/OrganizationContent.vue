<template>
  <div v-if="OrgId !== null" class="col-8">
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
      :OrgId="OrgId"
      @hideBoardForm="hideBoardForm"
      @addBoard="addBoard"
    ></board-form>
    <hr />
    <board-item
      v-for="(board, index) in boardsList"
      :key="board.id"
      :board="board"
      :index="index"
      @openBoard="openBoard"
    ></board-item>
  </div>
</template>

<script>
import BoardForm from './BoardForm';
import InviteForm from './InviteForm';
import MemberList from './MemberList';
import BoardItem from './BoardItem';

export default {
  components: { InviteForm, BoardForm, MemberList, BoardItem },
  name: 'OrganizationContent',
  props: ['OrgId', 'membersList', 'boardsList'],
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
    addBoard(body) {
      this.$emit('addBoard', body);
    },
    openBoard(b) {
      this.$emit('openBoard', b);
    },
  },
};
</script>

<style></style>
