<template>
  <div id="app">
    <h2>TODO List</h2>
    <input id = 'add-input' v-model = "newItem" v-on:keyup.enter = "addNew" placeholder="do what?" />
    <ul>
      <li require-v-for= "item in items">
        <h3 v-on:mouseenter = "itemEnter(item)" v-on:mouseleave =  "itemLeave(item)">
        <input type = 'checkbox' v-on:click = 'itemCheck(item)' />
        <p class="item-label" v-bind:class = "{'line-through':item.checked}">{{$index+1}}.{{item.label}}</p>
        <p class="item.status" v-if = "item.checked">finished</p>
        <p class="item.delete" v-if = "item.showDelete" v-on:click = "deleteClick(item)">delete</p> 
        </h3>
      </li>
    </ul>
  </div>
</template>
<style>
body {
  font-family:Arial, Helvetica, sans-serif;
}
#app {
  width: 800px;
  margin: 30px auto;
}
#add-input{
  width: 750px;
  height: 35px;
  padding: 0 5px;
}
ul{
  list-style: none;
  padding: 0;
}
li{
  height: 30px;
}
.item-status{
  display: inline;
  background: red;
  color: white;
  padding: 0 5px;
  font-size: 12px;
}
.item-delete{
  text-decoration: underline;
  display: inline;
  font-size: 12px;
  color: gray;
  cursor: pointer;
}
.item-label{
  display: inline;
}
.line-through{
  text-decoration: line-through;
}
</style>

<script>
import Store from './store'
export default {
    data: function () {
    return {
      items:Store.fetch(),
      newItem: '' 
    }
  },
  watch:{
    items:{
      handler:function(items){
        Store.save(items)
      },
      deep:true
    }
  },
  methods:{
    addNew(){
      this.items.push({
        label:this.newItem,
        checked:false,
        showDelete:false,
      })
      this.newItem = ''
      Store.save 
    },
    itemCheck(item){
      itemChecked = !item.checked
    },
    itemEnter(item){
      item.showDelete = true
    },
    itemLeave(item){
      item.showDelete = false
    },
    delelteClick(item){
      this.items.$remove(item)
    }
  }
}
</script>


