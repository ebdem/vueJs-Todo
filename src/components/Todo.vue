<template>
  <div class="row">
    <div
      class="col-md-6 offset-md-3 pt-2 pb-2 border d-flex flex-row align-items-center justify-content-between"
    >
      <span>{{ todo.text }}</span>
      <button class="btn btn-danger" @click="$emit('deleteTodo', todo.id)">
        Sil
      </button>
    </div>
  </div>
</template>
<script>
export default {
  props: ["todo"]
};
</script>
