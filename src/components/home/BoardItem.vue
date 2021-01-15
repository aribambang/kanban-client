<template>
  <div class="card mb-3" @click="openBoard(board.id, board.name)">
    <div class="card-body">
      <div>
        <h5 class="card-title">{{ index + 1 }}. {{ board.name }}</h5>
        <h6 class="card-subtitle mb-2 text-muted">
          <i class="fa fa-calendar" aria-hidden="true"></i>
          {{ moment(board.createdAt).format('DD MMMM YYYY') }}
        </h6>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
export default {
  name: 'BoardItem',
  props: ['index', 'board'],
  data: () => ({
    moment: moment,
  }),
  methods: {
    openBoard(id, name) {
      const b = { id, name };
      this.$emit('openBoard', b);
    },
  },
};
</script>

<style></style>
