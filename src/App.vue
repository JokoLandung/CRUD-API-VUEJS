<template>
  <div>
    <form @submit.prevent="add">
        <input type="hidden" v-model="form.id">
        <input type="text" v-model="form.name">
        <input type="text" v-model="form.price">
        <button type="submit" v-show="!updateSubmit">add</button>  
        <button type="button" v-show="updateSubmit" @click="update(form)">Update</button> 
    </form>
    <ul v-for="car in cars" :key="car.id">
      <li>
      <span>{{car.name}}</span> &#160;
      <span>{{car.price}}</span> &#160;
      <button @click="edit(car)">Edit</button> ||  <button @click="del(car)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
/* eslint-disable */ 
import axios from 'axios'

export default {
  data(){
    return{
        form: {
          id: '',
          name: '',
          price: ''
        },
        cars: '',
        updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://localhost:3000/cars').then(res => {
        this.cars = res.data
      }).catch ((err) => {
        console.log(err);
        
      })
    },
      add(){
      axios.post('http://localhost:3000/cars/', this.form).then(res => {
          this.load()
          this.form.name = '',
          this.form.price = ''
      })
    },
    edit(car){ 
        this.updateSubmit = true
        this.form.id = car.id 
        this.form.name = car.name 
        this.form.price = car.price
    },
    update(form){ 
       return axios.put('http://localhost:3000/cars/' + form.id , {name: this.form.name,price: this.form.price }).then(res => {
        this.load()
        this.form.id = ''
        this.form.name = ''
        this.form.price = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
        
      })
    },
    del(car){
      axios.delete('http://localhost:3000/cars/' + car.id).then(res =>{
          this.load()
          let index = this.cars.indexOf(form.name)
          this.cars.indexOf(form.name)
          this.cars.splice(index,1)
      })
    }
  }
}
</script>