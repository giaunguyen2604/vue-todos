<template>
  <div class="item-todo" v-bind:id="singleTodo.id">
    <div class="main-todo">
      <input type="checkbox" @change="checkChange($event)" v-model="singleTodo.checked"/>
      <p v-bind:class="{ 'item-checked': singleTodo.checked }">
        {{ singleTodo.content }}
      </p>
    </div>
    <button @click="deleteItem">Delete</button>
  </div>
</template>

<script>
export default {
  name: "ItemTodo",
  props: {
    singleTodo: {
      type: Object,
    },
    index: {
      type: Number,
    },
  },
  methods: {
    checkChange(e) {
      if (e.target.checked) {
        this.isChecked = true;
        this.$emit("increase", this.index);
      } else {
        this.isChecked = false;
        this.$emit("decrease", this.index);
      }
    },
    deleteItem() {
      this.$emit("deleteItem", this.index, this.isChecked);
    },
  },
};
</script>

<style scoped>
.item-todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 23px;
}

.item-checked {
  text-decoration: line-through;
}

.main-todo {
  display: flex;
  align-items: center;
}

input[type="checkbox"] {
  width: 1em;
  height: 1em;
  margin-top: 0.25em;
  vertical-align: top;
  background-color: #fff;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  border: 1px solid rgba(0, 0, 0, 0.25);
  appearance: none;
  color-adjust: exact;
  border-radius: 0.25em;
}

input[type="checkbox"]:checked {
  outline: none;
  background-color: #0d6efd;
  border-color: #0d6efd;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 20 20'%3e%3cpath fill='none' stroke='%23fff' stroke-linecap='round' stroke-linejoin='round' stroke-width='3' d='M6 10l3 3l6-6'/%3e%3c/svg%3e");
}

</style>

