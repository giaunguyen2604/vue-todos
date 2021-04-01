<template>
  <div class="item-todo" v-bind:id="singleTodo.id">
    <div class="main-todo">

      <template v-if="isEdit">
        <input type="text" :ref="'editInput'+singleTodo.id" :value="singleTodo.content"/>
      </template>

      <template v-else>
        <input
          type="checkbox"
          @change="checkChange($event)"
          v-model="singleTodo.checked"
        />
        <p v-bind:class="{ 'item-checked': singleTodo.checked }">
          {{ singleTodo.content }}
        </p>
      </template>

    </div>

    <div v-if="isEdit">
      <button @click="saveEdit" class='btn-primary'>Save</button>
      <button @click="toggleMode" :style="{ 'margin-left': '5px' }">
        Cancel
      </button>
    </div>

    <div v-else>
      <button @click="toggleMode" class='btn-primary'>Edit</button>
      <button @click="deleteItem" :style="{ 'margin-left': '5px' }" class='btn-danger'>
        Delete
      </button>
    </div>

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
  data() {
    return {
      isEdit: false,
    };
  },
  updated() {
    const element = this.$refs['editInput'+this.singleTodo.id]
    if (element) element.focus();
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
    toggleMode() {
      this.isEdit = !this.isEdit;
    },
    saveEdit() {
      const content =  this.$refs['editInput'+this.singleTodo.id].value;
      if (content) {
        this.$emit("updateList", this.index, content)
        this.toggleMode();   
      }
    }
  },
};
</script>

<style scoped>
p {
  margin: 0
}
.item-todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 23px;
  margin: 10px 0;
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

