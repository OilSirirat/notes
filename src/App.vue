<template>
  <div>
    <div class="hero-body">
      <h1 class="icon">
      NOTE
      </h1>
    </div>
  <div class="note1">

  <div class="container is-fullhd">
<div class="control">



</br></br>
<p class="add">
  </br>
<center><h1>Add your notes here</h1></center>
  </br>
<input class="input" type="text" name="header" value=""  v-model="data.header" placeholder="เรื่อง" size="34"  required=""></br>
<input class="input" type="date" name="" value="" v-model="data.date" required=""><br><br>
 <textarea class="inputarea" oninput="" name="mess" v-model="data.mess" required=""> ..NOTE.. </textarea></br>
 <center><button class="button is-success" @click="insert()" >SAVE</button></center></br>
</p>
</div>
</div>
</div></br>
<article>
<div class="sh" v-for ="(show, key) in note"><!--เรียกข้อมูลออกมาดู-->
  <center>
    <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          {{show.header}}
        </p>
      </header>
      <div class="card-content">
        <div class="content" v-if="checkEdit !== key">
          {{show.date}}
          <br>
          {{show.mess}}
        </div>
        <div class="content" v-else>
          <input type="date" name="" value="" v-model="show.date">
          <br>
          <input type="textarea" name="" value="" v-model="show.mess">
        </div>
      </div>
      <footer class="card-footer">
        <a class="card-footer-item" @click="swap(key)" v-if="checkEdit !== key" >Edit</a>
        <a class="card-footer-item" @click="update(key,show.date,show.mess)" v-else >save</a>
        <a class="card-footer-item" @click="deleteData(key)">Delete</a>
      </footer>
    </div>

</center>
<br>
</div>
</article>
</div>

</template>

<script>
// Initialize Firebaseง
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyCJwf1-ILG365Gnyw3JjbHdRb6_DpB4fBg',
  authDomain: 'note-85f36.firebaseapp.com',
  databaseURL: 'https://note-85f36.firebaseio.com',
  projectId: 'note-85f36',
  storageBucket: 'note-85f36.appspot.com',
  messagingSenderId: '1021476333131'
}
var firebaseApp = firebase.initializeApp(config)
var db = firebaseApp.database()
var getData = db.ref('/')
export default {
  name: 'app',
  firebase: {
    datas: []
  },
  data () {
    return {
      count: 0,
      data: {
        mess: '',
        header: '',
        date: ''
      },
      note: [],
      datas: getData,
      checkEdit: ''
    }
  },
  methods: {
    add: function () {
      this.count++
    },
    deleteData: function (key) {
    console.log(key)
    db.ref('/').child(key).remove()
    },
    swap: function (data) {
      this.checkEdit = data
    },
    update: function (key,date,mess) {
    console.log(key)
    db.ref('/').child(key).update({
      date:date,
      mess:mess
    });
    this.checkEdit = ''
    },
    insert: function () {
      if (this.mess !== '' && this.header !== '' && this.date !== '') {
        db.ref(this.header).push(this.data)
        this.data.mess = ''
        this.data.date = ''
        this.data.header = ''
      }
    }
  },
  mounted () {
    var vm = this
    // vm.$bindAsArray('note-85f36', db.ref('note-85f36'))
    let notes = db.ref('/')
    notes.on('value', function (snap) {
      console.log(snap)
      vm.note = snap.val()
    })
  }
}
</script>

<style>
.btn {
    border: none; /* Remove borders */
    color: white; /* Add a text color */
    padding: 14px 28px; /* Add some padding */
    cursor: pointer; /* Add a pointer cursor on mouse-over */
}
.note1 {
  display: inline-block;
  margin: 10 10px;
  margin-left: 10%;
}
o{
  margin-left: 41%;
}
.default {background-color: #e7e7e7; color: black;} /* Gray */
.default:hover {background: #ddd;}
.add {

    background-color: #f1f8e9;  /*สีเตรงเพิ่มstory*/
}
.sh{
   display: inline-block;
   width: 35%;
   margin: 5%;
}
.jumbotron {

  padding: 150px ;
  background: url(./assets/bb1.jpg)  ;
  background-size: 100%;
  color: #fff;

}
article {
    margin-left: 500px;
    margin-top: -32%;
    border-left: 0px solid gray;
    padding: 0em;
    overflow: hidden;
}
.inputarea {
  resize: none;
  width: 70%;
  height: 35vh;
  margin-left: 15%;
}
.hero-body {
  height: 10vh;
  background-color: #f48fb1;  /*สีบาร์*/
}
.icon {
  color: white;
  font-size: 30px;
  margin-left: 5%;
}
.card-header {
  background-color: #ffcdd2;  /*สีเฮร์ดเดอร์การ์ด*/
}
</style>
