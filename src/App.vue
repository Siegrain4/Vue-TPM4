<template>
  <div>
    <h1>Todo List</h1>
    <div>
      <input type="text" v-model="newTodo" placeholder="Tambahkan todo">
      <button @click="addTodo">Tambah</button>
    </div>
    <ul>
      <li v-for="(todo, index) in filteredTodos" :key="index">
        <span v-if="!todo.edited">{{ todo.text }}</span>
        <input v-else type="text" v-model="todo.text">
        <button @click="toggleEdit(index)">{{ todo.edited ? 'Simpan' : 'Edit' }}</button>
        <button @click="deleteTodo(index)">Hapus</button>
      </li>
    </ul>
    <p v-if="isHebat">Hebat!</p>
    <button class="reset-button" @click="resetTodos">Reset</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      showSuccessMessage: false
    };
  },
  computed: {
    isHebat() {
      return this.activeTodos.length >= 4;
    },
    isTodoEmpty() {
      return this.newTodo.trim() === '';
    },
    filteredTodos() {
      return this.todos.filter(todo => !todo.deleted);
    },
    activeTodos() {
      return this.todos.filter(todo => !todo.deleted);
    }
  },
  methods: {
    addTodo() {
      if (!this.isTodoEmpty) {
        this.todos.push({ id: Date.now(), text: this.newTodo.trim(), edited: false, deleted: false });
        this.newTodo = '';
      }else{
        alert("Data tidak boleh kosong!")
      }
    },
    toggleEdit(index) {
      const todo = this.todos[index];
      if (todo.edited) {
        todo.edited = false;
        alert("Data berhasil di Ubah!");
      } else {
        this.todos.forEach((item) => (item.edited = false));
        todo.edited = true;
      }
    },
    deleteTodo(index) {
      if (confirm('Anda yakin ingin menghapus todo ini?')) {
        this.todos[index].deleted = true;
        alert("Data berhasil di Hapus!");
      }
    },
    resetTodos() {
      this.todos = [];
    }
  }
};
</script>

<style scoped>
ul {
  list-style-type: decimal;
  margin-left: 20px;
  margin-top: 10px;
}

li {
  margin-bottom: 8px;
}

button {
  margin-left: 10px;
}

p {
  margin-top: 10px;
}

.reset-button {
  margin-top: 20px;
}
</style>
