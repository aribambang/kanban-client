<template>
  <div class="card p-2 mb-3" v-if="!updateFormActive && !moveActive">
    <h5>{{ task.title }}</h5>
    <p class="card-text">
      <small class="text-muted"
        ><i class="fa fa-calendar" aria-hidden="true"></i>
        {{ moment(task.createdAt).format('DD MMMM YYYY, HH:m') }}</small
      ><br />
      <small class="text-muted"
        ><i class="fa fa-user" aria-hidden="true"></i> {{ fullname }}</small
      >
    </p>
    <div v-if="Number(id) === task.UserId">
      <span class="text-muted"
        ><a class="mr-2" href="#" @click="showUpdateForm"
          ><i class="fa fa-edit text-primary" aria-hidden="true"></i></a
        ><a class="mr-2" href="#" @click="showMove"
          ><i class="fa fa-arrows text-muted" aria-hidden="true"></i
        ></a>
        <a href="#" @click="deleteTask"
          ><i class="fa fa-trash text-danger" aria-hidden="true"></i
        ></a>
      </span>
    </div>
  </div>
  <div v-else-if="updateFormActive && !moveActive" class="card p-2 mb-3">
    <form>
      <textarea
        cols="26"
        rows="4"
        placeholder="Enter title task"
        style="resize: none"
        v-model="title"
      ></textarea>
      <div class="mt-2">
        <div type="submit" @click="updateTask" class="btn btn-primary btn-sm">
          Update
        </div>
        <div @click="hideUpdateForm" class="btn btn-warning btn-sm">cancel</div>
      </div>
    </form>
  </div>
  <div v-else-if="moveActive && !updateFormActive" class="card p-2 mb-3">
    <form>
      <label class="mr-sm-2" for="inlineFormCustomSelect">Move To</label>
      <select
        v-model="selectedCategory"
        class="custom-select mr-sm-2"
        id="inlineFormCustomSelect"
      >
        <option
          v-for="cat in taskCategoriesList"
          :key="cat.id"
          :value="cat.id"
          >{{ cat.name }}</option
        >
      </select>
      <div class="mt-2">
        <div
          type="submit"
          @click="changeCatTask"
          class="btn btn-primary btn-sm"
        >
          Move
        </div>
        <div @click="hideMove" class="btn btn-warning btn-sm">
          cancel
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import moment from 'moment';
import Swal from 'sweetalert2';

export default {
  name: 'TaskItem',
  props: ['task', 'index', 'category', 'taskCategoriesList'],
  data() {
    return {
      id: localStorage.getItem('id'),
      moment: moment,
      updateFormActive: false,
      moveActive: false,
      title: this.task.title,
      selectedCategory: this.category.id,
    };
  },
  methods: {
    showUpdateForm() {
      this.updateFormActive = true;
    },
    hideUpdateForm() {
      this.updateFormActive = false;
    },
    showMove() {
      this.moveActive = true;
    },
    hideMove() {
      this.moveActive = false;
    },
    updateTask() {
      const body = {
        title: this.title,
      };
      const idTask = this.task.id;
      this.$emit('updateTask', { body, idTask });
      this.hideUpdateForm();
    },
    changeCatTask() {
      this.$emit('changeCatTask', {
        id: this.task.id,
        idCat: this.selectedCategory,
      });
      this.hideMove();
    },
    deleteTask() {
      Swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!',
      }).then((result) => {
        if (result.isConfirmed) {
          this.$emit('deleteTask', this.task.id);
          Swal.fire('Deleted!', 'Your task has been deleted.', 'success');
        }
      });
    },
  },
  computed: {
    fullname: function() {
      return `${this.task.User.first_name} ${this.task.User.last_name}`;
    },
  },
};
</script>

<style></style>
