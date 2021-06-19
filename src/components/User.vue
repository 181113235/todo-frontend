<template>
 
 <div>
  <h1>Daftar User</h1>
  <ul>
    <li v-for="item in users" :key="item.id">{{ item.username }}<button @click="hapus(item.id)">X</button></li>
  </ul>
  <input v-model="username"/>
  <input type=password v-model="password"/>
  <button @click="tambah">Add</button>
 </div>
</template>

<script>
import axios from 'axios'
export default {
  data:function() {
    return {
      users: [],
      username: '',
      password: ''
    }
  },
  created: function () {
    const username = localStorage.getItem('usr')
    const password = localStorage.getItem('pwd')
    axios.get('http://localhost:3080/user', { headers: {username, password}})
    .then(result=>{
      this.users = result.data   
    })
  },
  methods: {
    tambah: function (){
      const newItem = { username: this.username, password: this.password}
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.post('http://localhost:3080/user', newItem, { headers: {username, password}})
        .then((res)=>{
          //this.users.push(newItem)
          this.users.push({id:res.data.id, username: this.username, password: this.password})
          //location.reload()
        })
    },
    hapus: function (id) {
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.delete(`http://localhost:3080/user/${id}`, { headers: {username, password}})
        .then(()=>{
          for(var i = 0; i<this.users.length; i++){
            if(this.users.length == 1){
              alert("Usename terakhir tidak diperbolehkan untuk dihapus")
            }
            else if(this.users[i].id == id) {
              this.users.splice(i,1)
            }
          }
          
        })
    }
  }
}
</script>