<template>
  <div class='hello'>
    <p class='date'>Order Date: {{ orderDate }}</p>
    <input
      v-model='newItem.name'
      placeholder='Item Name'
    >
    </br>
    <input
      v-model='newItem.size'
      placeholder='Item Size'
    >
    </br>
    <input
      v-model='newItem.color'
      placeholder='Item Color'
    >
    </br>
    <input
      v-model='newItem.number'
      placeholder='Item Number'
    >
    </br>
    <button v-on:click='addNewItem'>Add Item</button>
    <table>
    <thead>
      <tr>
        <th>#</th>
        <th>Item Name</th>
        <th>Item Size</th>
        <th>Item Color</th>
        <th>Item Number</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for='(item, index) in items'
        :key="item['.key']"> 
        <td>{{ index + 1 }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.size }}</td>
        <td>{{ item.color }}</td>
        <td>{{ item.number }}</td>
        <button v-on:click='deleteitem(item)'>X</button>
      </tr>
    </tbody>
  </table>
  </div>
</template>

<script>
// var VueFire = require('vuefire')
var firebase = require('firebase')
var db = firebase.initializeApp({
  apiKey: 'AIzaSyCbA72K4sZKVYIwVxWTi58KMfc3CeAamqU',
  authDomain: 'myorderlistv2.firebaseapp.com',
  databaseURL: 'https://myorderlistv2.firebaseio.com',
  projectId: 'myorderlistv2',
  storageBucket: 'myorderlistv2.appspot.com',
  messagingSenderId: '795673083249'
}).database()
var orderYear = new Date().getFullYear()
var orderMonth = new Date().getMonth() + 1
var orderDay = new Date().getDate()
var itemsRef = db.ref('items/' + orderYear + orderMonth + orderDay)
export default {
  name: 'hello',
  data () {
    return {
      orderDate: new Date().toLocaleDateString(),
      id: 1,
      newItem: {
        name: '',
        size: '',
        color: '',
        number: ''
      }
    }
  },
  firebase: {
    items: itemsRef
  },
  methods: {
    addNewItem: function () {
      itemsRef.push(this.newItem)
      this.newItem.name = ''
      this.newItem.size = ''
      this.newItem.color = ''
      this.newItem.number = ''
    },
    deleteitem: function (item) {
      itemsRef.child(item['.key']).remove()
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
body {
  font-family: Helvetica Neue, Arial, sans-serif;
  font-size: 14px;
  color: #444;
}

table {
  border: 2px solid #42b983;
  border-radius: 3px;
  background-color: #fff;
}

th {
  background-color: #42b983;
  color: rgba(255,255,255,0.66);
  cursor: pointer;
}

td {
  background-color: #f9f9f9;
  text-align: center
}

th, td {
  min-width: 120px;
  padding: 10px 20px;
}

th.active {
  color: #fff;
}

th.active .arrow {
  opacity: 1;
}

.arrow {
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.66;
}

.arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid #fff;
}

.arrow.dsc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid #fff;
}

.date {
  font-weight: bold;
}
</style>
