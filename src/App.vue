<template>
  <div id="app">
    <div id="title">
       <h1 class="p-3  text-center">MANAGE PRODUCTS </h1>
    </div>

   <div class="container">
     
    <h3 class=" text-center">search by category</h3> 
    <select @change="onChangeCategory($event)">
         <option  value="all">all</option>
         <option v-for="item in category" :key="item" >{{item}}
         </option>
    </select>
    <div>
      Sorting By: <b>{{ sortBy }}</b>, Sort Direction:
      <b>{{ sortDesc ? 'Descending' : 'Ascending' }}</b>
    </div>
    

        
      <b-table class="table table-striped table-bordered"   :items="product"  :per-page="perPage"  :current-page="currentPage" 
      :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc" 
      :fields="fields"
      sort-icon-left 
         hover 
      >
       
      <template #cell(desscription)="data"  >
        <p>{{ data.value }}</p>   
      </template>

      <template #cell(rating)="data">
        {{ data.value.rate }} {{ data.value.count }}   
      </template>

      <template #cell(image)="data">
       <img :src="data.value " width="200" height="200">
      </template>
          
      </b-table>

      <b-pagination   align="center" v-model="currentPage"  :total-rows="rows"  :per-page="perPage"  aria-controls="my-table"  >
      <template #first-text><span class="text-success">First</span></template>
      <template #prev-text><span class="text-danger">Prev</span></template>
      <template #next-text><span class="text-warning">Next</span></template>
      <template #last-text><span class="text-info">Last</span></template>
      <template #ellipsis-text>
        <b-spinner small type="grow"></b-spinner>
        <b-spinner small type="grow"></b-spinner>
        <b-spinner small type="grow"></b-spinner>
      </template>
      <template #page="{ page, active }">
        <b v-if="active">{{ page }}</b>
        <i v-else>{{ page }}</i>
      </template>
      </b-pagination>
      <p class="mt-3">Current Page: {{ currentPage }}</p>

      
   </div>
  </div>
</template>

<script>
   

import axios from 'axios'
export default {
  name: 'App',
  components: {
  },

  data() {
        return {
          product: [],
          category: {},

          sortBy: 'price',
        sortDesc: false,
        fields: [
          { key: 'title', label:'title', sortable: false },
          { key: 'price',  label:'price', sortable: true },
          { key: 'description',  label:'description', sortable: false },
          { key: 'category',  label:'category', sortable: false },
          { key: 'image',  label:'image', sortable: false },
          { key: 'rating',  label:'rating', sortable: true }
        ],

        perPage: 5,
        currentPage: 1,
        }
  },
  computed: {
    rows() {
        return this.product.length
      }
  },
  mounted(){
    
    this.getproduct()
    this.getcategory()
 
  },
  
  

  methods: {
    getproduct(){
      
      axios.get("https://fakestoreapi.com/products").then((response) => {
      this.product = response.data
        },(error) => {
      console.log(error)
    })
    },

    
    getcategory(){
      axios.get("https://fakestoreapi.com/products/categories").then((response) => {
      this.category=response.data
      },(error) => {
      console.log(error)
      })
    },
    
    

    onChangeCategory(e) { 
      if(e.target.value=="all"){
        this.getproduct();

      }
      axios.get('https://fakestoreapi.com/products/category/' + e.target.value).then((response) => {
      this.product=response.data
      },(error) => {
      console.log(error)
      })
      console.log(e.target.value);
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
  margin-top: none  ;
  background-color: salmon;
}
#title {
  margin-left: none;
  margin-right: none;
  font-family:  cursive;
  text-align: center;
  color: salmon;
  background-color: #2c3e50;
}


</style>
