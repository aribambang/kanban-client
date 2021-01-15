<template>
  <div class="col-3">
    <div class="card maxh8">
      <div class="card-header">
        <div class="clearfix">
          <div class="pull-left">
            <b>{{ category.name }}</b>
          </div>
          <div class="pull-right">
            <a class="text-right" href="#" @click="deleteCategory"
              ><i class="fa fa-trash text-danger" aria-hidden="true"></i
            ></a>
          </div>
        </div>
      </div>
      <div class="card-body scrolling-wrapper">
        <task-item
          v-for="(task, index) in filterTask"
          :key="index"
          :task="task"
          :category="category"
          :taskCategoriesList="taskCategoriesList"
          @deleteTask="deleteTask"
          @updateTask="updateTask"
          @changeCatTask="changeCatTask"
        ></task-item>
      </div>
      <form>
        <div v-if="addTaskFormActive === true" class="card p-1">
          <textarea
            cols="30"
            rows="4"
            placeholder="Enter title task"
            style="resize: none"
            v-model="title"
          ></textarea>
        </div>
        <div v-if="addTaskFormActive === true" class="card-footer">
          <div type="submit" @click="addTask" class="btn btn-primary btn-sm">
            Add
          </div>
          <div @click="hideAddTask" class="btn btn-warning btn-sm">cancel</div>
        </div>
        <div v-else @click="showAddTask" class="card-footer">+ Add Task</div>
      </form>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import TaskItem from './TaskItem.vue';
import Swal from 'sweetalert2';
export default {
  components: { TaskItem },
  name: 'CategoryItem',
  props: ['category', 'index', 'taskCategoriesList'],
  data: () => ({
    moment: moment,
    addTaskFormActive: false,
    taskList: [],
    title: '',
  }),
  computed: {
    filterTask: function() {
      let d = this.category.Tasks.sort(function(a, b) {
        return new Date(a.createdAt) - new Date(b.createdAt);
      });
      return d;
    },
  },
  methods: {
    showAddTask() {
      this.addTaskFormActive = true;
    },
    hideAddTask() {
      this.addTaskFormActive = false;
    },
    addTask() {
      const body = {
        title: this.title,
        CategoryId: this.category.id,
      };
      this.$emit('addTask', body);
      this.title = '';
      this.hideAddTask();
    },
    deleteTask(id) {
      this.$emit('deleteTask', id);
    },
    changeCatTask(v) {
      this.$emit('changeCatTask', v);
    },
    updateTask(body) {
      this.$emit('updateTask', body);
    },
    deleteCategory() {
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
          this.$emit('deleteCategory', this.category.id);
        }
      });
    },
  },
};
</script>

<style scoped>
h1 {
  font-weight: 800;
  font-size: 2em;
}

.maxh8 {
  max-height: 500px;
}

.scrolling-wrapper {
  overflow-x: auto;
}
</style>
