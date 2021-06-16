<template>
 
 <div>
  <h1>Berikut adalah daftar tugas kita</h1>
  <ul>
    <li v-for="item in todos" :key="item.id">{{ item.deskripsi }}<button @click="hapus(item.id)">X</button></li>
  </ul>
  <input v-model="myText"/>
  <button @click="tambah">Add</button>
 </div>
</template>

<script>
import axios from 'axios'

export default {
  data:function() {
    return {
      todos: [],
      myText: ''
    }
  },
  created: function () {
    axios.get('http://localhost:3080/todo')
    .then(result=>{
      this.todos = result.data   
    })
  },
  methods: {
    tambah: function (){
      const newItem = {deskripsi: this.myText}
      axios.post('http://localhost:3080/todo', newItem)
        .then(()=>{
          this.todos.push(newItem)
          location.reload()
        })
      this.myText = ''
    },
    hapus: function (id) {
      axios.delete(`http://localhost:3080/todo/${id}`)
        .then(()=>{
          for(var i = 0; i<this.todos.length; i++){
            if(this.todos[i].id == id) {
              this.todos.splice(i,1)
            }
          }
          
        })
    }
  }
}
</script>