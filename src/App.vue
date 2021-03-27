<template>
  <div id="app">
    <div>
      Filter: <input type="text" v-model="filterText" />
    </div>
    <div class="vertical-space" v-if="!error">
      
      <table class="center">
        <thead>
          <tr>
            <th>No.</th>
            <th>Category</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in filterCategorys" :key="index">
            <td>{{ index + 1 }}</td>
            <td style="text-align: left;">{{ item }}</td>
          </tr>
        </tbody>
      </table>

    </div>
    <div class="vertical-space" v-else>
      {{ error }}
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data(){
    return {
      filterText: '',
      categorys: [],
      error: null
    }
  },
  computed:{
    filterCategorys: function(){
      let locFiltertext = this.filterText

      if(!locFiltertext) return this.categorys

      return this.categorys.filter( function(item) {
        return item.toLowerCase().includes(locFiltertext.toLowerCase())
      })
    }
  },
  async mounted(){
    this.error = null

    try{
      let { data: categoryResponse } = await axios.get('https://api.publicapis.org/categories')
      console.log(categoryResponse)
      this.categorys = categoryResponse
    }catch(err){
      console.log('Error')
      this.error = "Not get data category!!"
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

table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
  padding-left: 20px;
  padding-right: 20px;
}

.center {
  margin-left: auto;
  margin-right: auto;
}

.vertical-space {
  margin-top: 10px;
}
</style>
