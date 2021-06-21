<template>
  <section class="container">
    <h1>Todo App</h1>
    <p>
      <input type="text" name="content" v-model="content" @focus="set_flg" />
    </p>
    <div>
      <button @click="insert">save</button>
      <button @click="find">find</button>
    </div>
    <ul>
      <li v-for="(todo, index) in display_todos" :key="index">
        <span>{{ todo.content }}</span
        ><span>({{ todo.created }})</span><span @click="remove(todo)">×</span>
      </li>
    </ul>
  </section>
</template>

<script>
import { mapState } from "vuex";

export default {
  data: function() {
    return {
      content: "",
      find_flg: false
    };
  },
  computed: {
    ...mapState(["todos"]),
    display_todos: function() {
      if (this.find_flg) {
        var arr = [];
        var data = this.todos;
        data.forEach(element => {
          if (element.content.toLowerCase() == this.content.toLowerCase()) {
            arr.push(element);
          }
        });
        return arr;
      } else {
        return this.todos;
      }
    }
  },
  methods: {
    insert: function() {
      this.$store.commit("insert", { content: this.content });
      this.content = "";
    },
    find: function() {
      this.find_flg = true;
    },
    set_flg: function() {
      if (this.find_flg) {
        this.find_flg = false;
        this.content = "";
      }
    },
    remove: function(todo) {
      this.$store.commit("remove", todo);
    }
  }
};
</script>
<style>
.container {
  width: 400px;
  margin: 100px auto;
  text-align: center;
}

h1 {
  font-size: 32pt;
}

input {
  width: 300px;
  margin: 20px;
  padding: 8px 4px;
  font-size: 16pt;
}

button {
  margin: 0 10px;
  padding: 4px 8px;
  font-size: 10pt;
}

ul {
  margin-top: 20px;
  padding: 0;
  text-align: center;
}

li {
  list-style: none;
  margin: 5px 0;
  font-size: 14pt;
}

span {
  margin: 0 5px;
}
</style>
