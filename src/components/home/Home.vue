<template>
  <div id="home">
    <div class="container">
      <div class="row mt-5" id="projects">
        <div class="col">
          <h2 id="hello">Hi, {{ full_name }}</h2>
          <button class="btn btn-primary w-50 mt-3" @click="showOrgForm">
            Create Organization
          </button>
          <organization-form
            :orgFormActive="orgFormActive"
            :host="host"
            @hideOrgForm="hideOrgForm"
            @addOrg="addOrg"
            @showAlert="showAlert"
          ></organization-form>
          <table class="table table-striped mt-3">
            <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Organization</th>
                <th scope="col">Action</th>
              </tr>
            </thead>
            <tbody>
              <organization-list
                v-for="(org, index) in orgsList"
                :key="org.id"
                :org="org"
                :index="index"
                @deleteOrg="deleteOrg"
                @showOrg="showOrg"
              ></organization-list>
            </tbody>
          </table>
        </div>
        <organization-content
          ref="orgContent"
          :OrgId="OrgId"
          :membersList="membersList"
          @inviteMember="inviteMember"
          @deleteMember="deleteMember"
        ></organization-content>
      </div>
    </div>
  </div>
</template>

<script>
import OrganizationForm from './OrganizationForm';
import OrganizationList from './OrganizationList';
import OrganizationContent from './OrganizationContent';
export default {
  components: { OrganizationForm, OrganizationList, OrganizationContent },
  props: ['host', 'orgsList', 'membersList'],
  name: 'Home',
  data() {
    return {
      full_name: localStorage.getItem('full_name'),
      orgFormActive: false,
      OrgId: null,
    };
  },
  methods: {
    showOrgForm() {
      this.orgFormActive
        ? (this.orgFormActive = false)
        : (this.orgFormActive = true);
    },
    hideOrgForm() {
      this.orgFormActive = false;
    },
    showAlert({ t, m }) {
      this.$emit('showAlert', { t, m });
    },
    addOrg(body) {
      this.$emit('addOrg', body);
    },
    deleteOrg(id) {
      this.$emit('deleteOrg', id);
    },
    fetchOrganizations() {
      this.$emit('fetchOrganizations');
    },
    fetchMembers(id) {
      this.$emit('fetchMembers', id);
    },
    showOrg(id) {
      this.OrgId = id;
      if (this.OrgId !== null) {
        this.fetchMembers(id);
      }
    },
    hideInviteForm() {
      this.$refs.orgContent.hideInviteForm();
    },
    inviteMember(v) {
      this.$emit('inviteMember', v);
    },
    deleteMember(v) {
      this.$emit('deleteMember', v);
    },
  },
  mounted() {
    this.$emit('fetchOrganizations');
  },
};
</script>

<style></style>
