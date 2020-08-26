<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @addTodo="addTodo"></TodoInput>
    <TodoList :propsdata="todoItems" @removeTodo="removeTodo" @updateTodo="updateTodo"></TodoList>
    <TodoFooter @clearTodo="clearTodo"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoInput from './components/TodoInput'
import TodoList from './components/TodoList'
import TodoFooter from './components/TodoFooter'

export default {
  data() {
    return {
      todoItems: []
    }
  },
  methods: {
    addTodo(todoItem){
      const max_key = this.maxKey();
      localStorage.setItem(max_key,todoItem);
      this.todoItems.push({'t_key':max_key, 'item':todoItem});
    },
    removeTodo(index) {
      localStorage.removeItem(this.todoItems[index]['t_key']);
      this.todoItems.splice(index,1);
    },
    clearTodo() {
      localStorage.clear();
      this.todoItems = [];
    },
    updateTodo(index, newItem) {
      localStorage.removeItem(this.todoItems[index]['t_key']);
      localStorage.setItem(this.todoItems[index]['t_key'], newItem);
      this.todoItems[index]['item'] = newItem;
    },
    maxKey(){
      let max_key = 0
      for(var i =0; i < localStorage.length; i++) {
        let key_value = parseInt(localStorage.key(i));
        if(max_key < key_value){
          max_key = key_value;
        }
      }
      // 키는 MAX 값보다 1큰 값
      max_key++;

      if(max_key < 10) {
        max_key = '000' + max_key;
      }
      else if (max_key <100) {
        max_key = '00' + max_key;
      }
      else if (max_key < 1000) {
        max_key = '0' + max_key;
      }
      else {
        max_key = '' + max_key;
      }

      return max_key;
    },
    sortByKey(array, key) {
      return array.sort(function(a, b) {
          var x = a[key]; var y = b[key];
          return ((x < y) ? -1 : ((x > y) ? 1 : 0));
      })
    },
  },
  created() {
        if(localStorage.length >0) {
            for(var i=0; i < localStorage.length; i++) {
              let key = localStorage.key(i);
              if(key !=='loglevel:webpack-dev-server') {
                this.todoItems.push({'t_key':key,'item':localStorage.getItem(key)});
              }
              
            }
        }
        this.todoItems = this.sortByKey(this.todoItems,'t_key');
  },
  components:{
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter
  }
}
</script>

<style>
    body {
        text-align: center;
        background-color: #F6F6F8;
    }
    input {
      border-style: groove;
      width: 200px;
    }
    button {
      border-style: groove;
    }
    .shadow {
      box-shadow: 5px 10px 10px rgba(0,0,0,0.03);
    }

</style>
