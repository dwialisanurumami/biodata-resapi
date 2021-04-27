<template>
  <div>
    <h1>Form Biodata</h1>
    <form @submit.prevent="add">
      <label for="name">Nama Lengkap : </label>
          <input type="hidden" v-model="form.id">
          <input type="text" v-model="form.name" placeholder="nama">
      <br><br>
      <label for="jeniskelamin">Jenis Kelamin : </label>
          <input type="text" v-model="form.jeniskelamin" placeholder="jenis kelamin">
      <br><br>
      <label for="agama">Agama : </label>
          <input type="text" v-model="form.agama" placeholder="agama">
      <br><br>
      <label for="tanggallahir">Tanggal Lahir : </label>
          <input type="date" v-model="form.tanggallahir" placeholder="tanggal lahir">
      <br><br>
      <label for="telepon">Telepon : </label>
          <input type="number" v-model="form.telepon" placeholder="telepon">
      <br><br>
      <label for="email">Email : </label>
          <input type="text" v-model="form.email" placeholder="email">
      <br><br>
      <label for="alamat">Alamat : </label>
          <input type="text" v-model="form.alamat" placeholder="alamat">
          <button type="submit" v-show="!updateSubmit">add</button> <!-- jika tidak update maka tombol update tidak muncul --> 
          <button type="button" v-show="updateSubmit" @click="update(form)">Update</button> <!-- jika tombol edit di klik maka tombol add akan berubah menjadi update -->
    </form>
    <div>
      <h1>Tampilan : </h1>
        <table border="1">
        <tr>
          <th>Nama Lengkap</th>
          <th>Jenis Kelamin</th>
          <th>Agama</th>
          <th>Tanggal Lahir</th>
          <th>Telepon</th>
          <th>Email</th>
          <th>Alamat</th>
          <th>Action</th>
        <tr v-for="user in users" :key="user.id">
        <td>{{user.name}}</td>
        <td>{{user.jeniskelamin}}</td>
        <td>{{user.agama}}</td>
        <td>{{user.tanggallahir}}</td>
        <td>{{user.telepon}}</td>
        <td>{{user.email}}</td>
        <td>{{user.alamat}}</td>
        <button class="material-icons" @click="edit(user)">edit</button> || <button class="material-icons" @click="del(user)">delete</button>
        </tr>
        </table>
    </div>
  </div>         
</template>

  <script>
    import axios from 'axios'
export default {
  data(){
    return{
       form: {
          id: '',
          name: '',
          jeniskelamin: '',
          agama: '',
          tanggallahir: '',
          telepon: '',
          email: '',
          alamat: ''
        },
        users: '',
        updateSubmit: false
    }
  },
   mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://localhost:3000/users').then(res => {
        this.users = res.data //respon dari rest api dimasukan ke users
      }).catch ((err) => {
        console.log(err);
      })
    },
    add(){
      axios.post('http://localhost:3000/users/', this.form).then(res => {
          this.load()
          this.form.name = ''
          this.form.jeniskelamin = '',
          this.form.agama = '',
          this.form.tanggallahir = '',
          this.form.telepon = '',
          this.form.email = '',
          this.form.alamat = ''
      })
    },
    edit(user){ 
        this.updateSubmit = true
        this.form.id = user.id 
        this.form.name = user.name 
        this.form.jeniskelamin = user.jeniskelamin,
        this.form.agama = user.agama,
        this.form.tanggallahir = user.tanggallahir,
          this.form.telepon = user.telepon,
          this.form.email = user.email,
          this.form.alamat = user.alamat
    },
    update(form){
        return axios.put('http://localhost:3000/users/' + form.id, { name: this.form.name }).then(res =>{
          this.load()
        this.form.id = ''
        this.form.name = ''
        this.form.jeniskelamin = ''
        this.form.agama = ''
        this.form.tanggallahir = ''
        this.form.email = ''
        this.form.alamat = ''
        this.updateSubmit= false 
      }).catch((err) => {
        console.log(err);
      })
    },
    del(user){
      axios.delete('http://localhost:3000/users/' + user.id).then(res =>{
          this.load()
          let index = this.users.indexOf(form.name)
          this.users.splice(index,1)
      })
    }
   }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>