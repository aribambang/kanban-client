<template>
  <div class="col-3">
    <div class="card maxh8">
      <div class="card-header">
        <b>{{ category.name }}</b>
      </div>
      <div class="card-body scrolling-wrapper">
        <task-item
          v-for="(task, index) in category.Tasks"
          :key="index"
          :task="task"
          :category="category"
          @deleteTask="deleteTask"
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
    filterCategory: function() {},
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
