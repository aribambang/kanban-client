<template>
  <div v-if="boardFormActive === true" class="card" id="projectTodoForm">
    <div class="card-body">
      <form onsubmit="addTodo(event, 1)">
        <input type="hidden" id="pProjectIdTodo" />
        <div class="form-group">
          <input
            type="text"
            class="form-control"
            id="pTitleTodo"
            placeholder="Enter Board Name"
          />
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
</template>

<script>
export default {
  name: 'BoardForm',
  props: ['boardFormActive'],
};
</script>

<style></style>
