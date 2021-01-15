<template>
  <div
    class="card p-2 mb-3"
    :style="[
      Number(id) === task.UserId
        ? 'background-color: blue;'
        : 'background-color: green;',
    ]"
  >
    <h5>{{ task.title }}</h5>
    <p class="card-text">
      <small class="text-muted"
        ><i class="fa fa-calendar" aria-hidden="true"></i>
        {{ moment(task.createdAt).format('DD MMMM YYYY') }}</small
      ><br />
      <small class="text-muted"
        ><i class="fa fa-user" aria-hidden="true"></i> {{ fullname }}</small
      >
    </p>
    <div v-if="Number(id) === task.UserId">
      <span class="text-muted"
        ><a class="mr-2" href="#"
          ><i class="fa fa-edit text-primary" aria-hidden="true"></i></a
        ><a class="mr-2" href="#"
          ><i class="fa fa-arrows text-muted" aria-hidden="true"></i
        ></a>
        <a href="#" @click="deleteTask"
          ><i class="fa fa-trash text-danger" aria-hidden="true"></i
        ></a>
      </span>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import Swal from 'sweetalert2';

export default {
  name: 'TaskItem',
  props: ['task', 'index', 'category'],
  data() {
    return {
      id: localStorage.getItem('id'),
      moment: moment,
    };
  },
  methods: {
    deleteTask() {
      // console.log(this.task.id);
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
