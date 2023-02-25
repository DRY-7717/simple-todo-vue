<template>
  <ul class="list-group mt-4">
    <li
      class="list-group-item border-0 shadow-sm p-3 d-flex justify-content-between align-items-center mb-3"
      v-for="(todo) in list"
      :key="todo"
    >
      <del v-if="todo.isDone">{{ todo.activity }}</del>
      <p class="m-0" v-else>{{ todo.activity }}</p>
      <div class="box-btn">
        <button class="btn btn-sm btn-success me-2" @click="donetodo(index)">
          Done
        </button>
        <button class="btn btn-sm btn-danger" @click="deletetodo(index)">
          Delete
        </button>
      </div>
    </li>
  </ul>
</template>
<script>
export default {
  props: {
    list: {
      type: Object,
      default: {},
    },
  },
  setup(props, { emit }) {
    const deletetodo = (val) => {
      emit("deltodo", val);
    };
    const donetodo = (val) => {
      emit("dontodo", val);
    };

    return {
      deletetodo,
      donetodo,
    };
  },
};
</script>
