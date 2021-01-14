<template>
  <div id="board" v-if="board !== null">
    <div class="container-fluid">
      <h1 class="mt-3">{{ board.name }}</h1>
      <div class="scrolling-wrapper row flex-row flex-nowrap ">
        <category-item
          v-for="(category, index) in boardCategories.Categories"
          :category="category"
          :index="index"
          :key="category.id"
          @addTask="addTask"
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
  props: ['board', 'boardCategories'],
  methods: {
    addTask(body) {
      this.$emit('addTask', body);
    },
    hideAddTask() {
      this.$refs.cItem.hideAddTask();
    },
  },
};
</script>

<style scoped>
.scrolling-wrapper {
  overflow-x: auto;
}
</style>
