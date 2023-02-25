<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-lg-8 mx-auto mt-5">
          <div class="card border-0 shadow">
            <div class="card-body">
              <h3 class="card-title">Simple Todo App</h3>
              <h5 class="text-muted">Total Todo: {{ totaltodo }}</h5>
              <div class="row mt-4">
                <div class="col-12 col-sm-9 col-md-10 col-lg-10">
                  <input
                    type="text"
                    v-model="text"
                    class="form-control"
                    placeholder="write your activity"
                    @keyup.enter="addtodo"
                  />
                </div>
                <div class="col-12 col-sm-3 col-md-2 col-lg-2">
                  <button
                    class="btn btn-primary w-100 w-lg-0 mt-3 mt-sm-0 mt-md-0 mt-lg-0"
                    @click="addtodo"
                  >
                    Submit
                  </button>
                </div>
              </div>
              <List :list="list" @deltodo="deletetodo" @dontodo="donetodo" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { ref, reactive, toRefs, computed, onMounted } from "vue";
import List from "./components/BuildComponent.vue";

export default {
  components: { List },

  setup(props) {
    const text = ref("");
    const todos = reactive({
      list: [],
    });

    onMounted(() => {
      if (localStorage.getItem("todos")) {
        todos.list = JSON.parse(localStorage.getItem("todos"));
      }
    });

    const addtodo = () => {
      todos.list.unshift({
        activity: text.value,
        isDone: false,
      });
      text.value = "";
      savetolocalstorage();
    };
    const deletetodo = (todoindex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoindex) {
          return item;
        }
      });
      savetolocalstorage();
    };
    const donetodo = (todoindex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoindex) {
          item.isDone = !item.isDone;
        }
        return item;
      });
      savetolocalstorage();
    };
    const totaltodo = computed(() => {
      return todos.list.length;
    });

    const savetolocalstorage = () => {
      localStorage.setItem("todos", JSON.stringify(todos.list));
    };

    return {
      text,
      ...toRefs(todos),
      addtodo,
      totaltodo,
      deletetodo,
      donetodo,
    };
  },
};
</script>
