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
              ></organization-list>
            </tbody>
          </table>
        </div>
        <div class="col-8" id="projectContent">
          <h3>Boards:</h3>
          <div class="mb-3">
            <button
              class="btn btn-primary w-25"
              onclick="showProjectTodoForm()"
            >
              Add Boards
            </button>
            <button class="btn btn-primary w-25" onclick="showMemberForm()">
              Invite Member
            </button>
          </div>
          <div class="card" id="memberForm">
            <div class="card-body">
              <form onsubmit="member(event, 1)">
                <input type="hidden" id="mProjectIdTodo" />
                <div class="form-group">
                  <input
                    type="text"
                    class="form-control"
                    id="mEmail"
                    placeholder="Enter Email"
                  />
                </div>
                <div class="form-group">
                  <button type="submit" class="btn btn-primary w-25">
                    Add
                  </button>
                  <button
                    type="button"
                    onclick="hideMemberForm()"
                    class="btn btn-danger w-25"
                  >
                    Close
                  </button>
                </div>
              </form>
            </div>
          </div>
          <div class="card" id="projectTodoForm">
            <div class="card-body">
              <form onsubmit="addTodo(event, 1)">
                <input type="hidden" id="pProjectIdTodo" />
                <div class="form-group">
                  <input
                    type="text"
                    class="form-control"
                    id="pTitleTodo"
                    placeholder="Enter Title"
                  />
                </div>
                <div class="form-group">
                  <input type="date" class="form-control" id="pDateTodo" />
                </div>
                <div class="form-group">
                  <textarea
                    class="form-control"
                    rows="5"
                    id="pDescriptionTodo"
                    placeholder="Enter Description"
                  ></textarea>
                </div>
                <div class="form-group">
                  <button type="submit" class="btn btn-primary w-25">
                    Add
                  </button>
                  <button
                    type="button"
                    onclick="hideProjectTodoForm()"
                    class="btn btn-danger w-25"
                  >
                    Close
                  </button>
                </div>
              </form>
            </div>
          </div>
          <hr />
          <div id="projectTodoList"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import OrganizationForm from './OrganizationForm';
import OrganizationList from './OrganizationList';
export default {
  components: { OrganizationForm, OrganizationList },
  props: ['host', 'orgsList'],
  name: 'Home',
  data() {
    return {
      full_name: localStorage.getItem('full_name'),
      orgFormActive: false,
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
  },
  mounted() {
    this.$emit('fetchOrganizations');
  },
};
</script>

<style></style>
