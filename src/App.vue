<template>
  <div id="app">
    <h1 v-html="title"></h1>
    <input v-model = "newItem" v-on:keyup.enter = "addNew">
    <ul>
      <li v-for="item in items" v-bind:class="{finished:item.isFinished}" v-on:click="finish(item)">
        {{item.label}}
      </li>
    </ul>
    <p> child tells me :{{childWords}}</p>
    <component-a msgfromfather ="happy!" aaa="aaa" v-on:child-tell-me-somthing = "listenTo"></component-a>
  </div>
</template>

<script>
import Store from "./store"
import ComponentA from "./components/componentA"
export default {
   data:function(){
    return{
      title:"this is a todolist",
      items:Store.fetch(),
      newItem:"",
      childWords:""
    }
   },
   components:{ComponentA},
   watch:{
    items:{
      handler:function(items){
        Store.save(items)
      },
      deep:true
    }
   },
   events:{"child-tell-me-somthing":function(msg){
    this.childWords = msg;
   }},
   methods:{
    finish:function(item){
      item.isFinished = !item.isFinished
    },
    addNew:function(){
      this.items.push({
        label:this.newItem,
        isFinished:false
      })
      this.newItem = "";
    },
    listenTo:function(msg){
      this.childWords = msg;
    }
   }
  
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.finished{
  text-decoration:underline;
}
</style>
