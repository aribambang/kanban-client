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
          @deleteCategory="deleteCategory"
          ref="cItem"
        ></category-item>
        <div class="col-3">
          <div>
            <form v-if="addCatActive">
              <div class="card p-1">
                <input
                  type="text"
                  class="form-control"
                  placeholder="Enter name category"
                  v-model="name"
                />
              </div>
              <div class="card-footer">
                <div
                  type="submit"
                  @click="addCategory"
                  class="btn btn-primary btn-sm"
                >
                  Add
                </div>
                <div @click="hideAddCatForm" class="btn btn-warning btn-sm">
                  cancel
                </div>
              </div>
            </form>
            <button
              v-else
              @click="showAddCatForm"
              type="button"
              class="btn btn-primary"
            >
              Add Category
            </button>
          </div>
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
  data() {
    return {
      addCatActive: false,
      name: '',
    };
  },
  props: ['board', 'taskCategoriesList'],
  methods: {
    showAddCatForm() {
      this.addCatActive = true;
    },
    hideAddCatForm() {
      this.addCatActive = false;
    },
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
    addCategory() {
      const body = {
        name: this.name,
        BoardId: this.board.id,
      };

      this.$emit('addCategory', body);
      this.name = '';
      this.hideAddCatForm();
    },
    deleteCategory(id) {
      this.$emit('deleteCategory', id);
    },
  },
};
</script>

<style scoped>
.scrolling-wrapper {
  overflow-x: auto;
}
</style>
