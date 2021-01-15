<template>
  <div id="board" v-if="board !== null">
    <div class="container-fluid">
      <h1 class="my-4">{{ board.name }}</h1>
      <div class="scrolling-wrapper row flex-row flex-nowrap ">
        <category-item
          v-for="(category, index) in taskCategoriesList"
          :category="category"
          :index="index"
          :key="category.id"
          :taskCategoriesList="taskCategoriesList"
          @addTask="addTask"
          @deleteTask="deleteTask"
          @updateTask="updateTask"
          @changeCatTask="changeCatTask"
          ref="cItem"
        ></category-item>
        <div class="col-2">
          <button type="button" class="btn btn-primary">
            Add Category
          </button>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <div class="m-3"><h4>Create or select Board</h4></div>
  </div>
</template>

<script>
import CategoryItem from './CategoryItem.vue';
export default {
  components: { CategoryItem },
  name: 'Board',
  props: ['board', 'taskCategoriesList'],
  methods: {
    addTask(body) {
      this.$emit('addTask', body);
    },
    deleteTask(id) {
      this.$emit('deleteTask', id);
    },
    updateTask(body) {
      this.$emit('updateTask', body);
    },
    hideAddTask() {
      this.$refs.cItem.hideAddTask();
    },
    changeCatTask(v) {
      this.$emit('changeCatTask', v);
    },
  },
};
</script>

<style scoped>
.scrolling-wrapper {
  overflow-x: auto;
}
</style>
