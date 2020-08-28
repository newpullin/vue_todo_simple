<template>
  <div id="app" v-bind:class= "{'night' : toggle_value}">
    <TodoHeader :toggle_value="toggle_value"></TodoHeader>
    <TodoSwitch @changed="toggleSwitch" ></TodoSwitch>
    <TodoInput @addTodo="addTodo"></TodoInput>
    <TodoList :propsdata="todoItems" :toggle_value="toggle_value" @removeTodo="removeTodo" @updateTodo="updateTodo"></TodoList>
    <TodoFooter @clearTodo="clearTodo"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoInput from './components/TodoInput'
import TodoList from './components/TodoList'
import TodoFooter from './components/TodoFooter'
import TodoSwitch from './components/TodoSwitch'

export default {
  data() {
    return {
      todoItems: [],
      toggle_value: false
    }
  },
  methods: {
    addTodo(todoItem){
      const max_key = this.maxKey();
      localStorage.setItem(max_key,todoItem);
      this.todoItems.push({'t_key':max_key, 'item': todoItem});
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
    toggleSwitch() {
      this.toggle_value = !this.toggle_value
    }
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
    'TodoFooter': TodoFooter,
    'TodoSwitch': TodoSwitch,
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
    html,
#app {
  min-height: 100%;
}
#app {
  padding: 15px;
  background-image: linear-gradient(to bottom, #FFFFFF 30%, #88D9FF, #24206C, #222 80%);
  color: #333;
  background-size: 100% 400%;
  background-position: 100% 0%;
  transition: all 1s;
}
#app.night {
  background-position: 100% 100%;
  color:#FFFFFF;
}
.toggle-box-label-left:empty {
  margin-left: -10px;
}
.toggle-box-label-left:before,
.toggle-box-label-left:after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  /*transition*/
  -webkit-transition: 0.25s ease-in-out;
  -moz-transition: 0.25s ease-in-out;
  -o-transition: 0.25s ease-in-out;
  transition: 0.25s ease-in-out;
  outline: none;
}
.toggle-box input[type=checkbox],
.toggle-box input[type=checkbox]:active {
  position: absolute;
  top: -5000px;
  height: 0;
  width: 0;
  opacity: 0;
  border: none;
  outline: none;
}
.toggle-box label {
  display: inline-block;
  position: relative;
  padding: 0px;
  margin-bottom: 20px;
  font-size: 14px;
  line-height: 16px;
  cursor: pointer;
  color: rgba(149, 149, 149, 0.51);
  font-weight: normal;
}
.toggle-box-label-left:before {
  content: '';
  display: block;
  position: absolute;
  z-index: 1;
  line-height: 34px;
  text-indent: 40px;
  height: 16px;
  width: 16px;
  margin: 4px;
  /*border-radius*/
  -webkit-border-radius: 100%;
  -moz-border-radius: 100%;
  border-radius: 100%;
  right: 26px;
  bottom: 0px;
  background: #FFB200;
  transform: rotate(-45deg);
  box-shadow: 0 0 10px white;
}
.toggle-box-label-left:after {
  content: "";
  display: inline-block;
  width: 40px;
  height: 24px;
  /*border-radius*/
  -webkit-border-radius: 16px;
  -moz-border-radius: 16px;
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.15);
  vertical-align: middle;
  margin: 0 10px;
  border: 2px solid #FFB200;
}
.toggle-box input[type=checkbox]:checked + .toggle-box-label-left:before {
  right: 17px;
  box-shadow: 5px 5px 0 0 #eee;
  background: transparent;
}
.toggle-box input[type=checkbox]:checked + .toggle-box-label-left:after {
  background: rgba(0, 0, 0, 0.15);
  border: 2px solid white;
}
.toggle-box input[type=checkbox] + .toggle-box-label-left {
  color: rgba(250, 250, 250, 0.51);
  font-weight: bold;
}
.toggle-box input[type=checkbox]:checked + .toggle-box-label-left {
  color: rgba(149, 149, 149, 0.51);
  font-weight: normal;
}
.toggle-box input[type=checkbox]:checked + .toggle-box-label-left + .toggle-box-label {
  color: rgba(250, 250, 250, 0.51);
  font-weight: bold;
}

</style>
