<template>
  <div id="app" class="grid">
    <div class="storebar">
      <h1>Store API</h1>
    </div>
    <aside class="filter-controls">
      <div>
        <div>
          <h3>Search</h3>
          <input v-model="query" type="text">
        </div>

        
        <fieldset >
          <h3>Categories</h3>
          <template v-for="category in category_list">
          
            <div :key="category">
              <input type="checkbox" :id="category" name="categories" :value="category" v-model="selected_categories">
              <label :for="category">{{category}}</label>
            </div>

          </template>
        </fieldset>
      </div>

    </aside>
    <main>
      <ul class="product-container">
        <product-card 
        v-for="product in searchedResults" 
        :key="'product_id_' + product.id"
        :product="product"
        v-on:show-card="selected_product_id = $event"
        >
          
        </product-card>
      </ul>
        <product-modal 
          v-if="selected_product_id"
          :product="showDetails"
          v-on:close="selected_product_id = null"
          >
          
        </product-modal>
    </main>

  </div>
</template>

<script>
import ProductCard from './components/ProductCard.vue'
import ProductModal from './components/ProductModal.vue'


export default {
  components: { ProductCard, ProductModal },
  name: 'App',
  data(){
    return {
      products: {},
      category_list: [],
      query: '',
      selected_categories: [],
      selected_product_id: null
    }
  },
  computed:{
    showDetails(){
      let product = null;
      if (this.selected_product_id !== null) {


        product = this.products.find(product => {
            return product.id === this.selected_product_id;
        });
      }

      return product;
    },
    searchedResults(){
      let results  = this.products;

      if (this.query !== '') {
        results  = results.filter(product => {
          let title = product.title.toLowerCase();
          let category = product.category.toLowerCase();
          let query = this.query.toLowerCase();
          return title.includes(query) || category.includes(query);
        });
      }

      if (this.selected_categories.length > 0 ) {
        results = results.filter(product => {
            let category = product.category.toLowerCase();
            return this.selected_categories.includes(category);

        });
      }
      return results;
    },
  },
  created(){

      fetch('https://fakestoreapi.com/products')
            .then(res=>res.json())
            .then(json=> {
              console.log(JSON)
              this.products = json;

              this.category_list = this.products.map(product => {
                  return product.category;
              }).reduce((accumulator, currentValue) => {
                if (accumulator.indexOf(currentValue) === -1) {
                    accumulator.push(currentValue)
                  }
                  return accumulator;
                }, []);


            }).catch(err=>{
                console.log(err)
            });
  }





}
</script>

<style>
.storebar {grid-column: 1 / -1; position:sticky; top:0px; background-color: white; color:#31d586; z-index: 9;     box-shadow: 0 0 9px -3px #c8c8c8}
h1 {margin: 0.5em;}
p {margin: 0;}
fieldset {border:none}

body {background-image: linear-gradient(#edfaf6 20%, white); background-repeat: no-repeat; margin:0}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #708a89;
}

.grid {
  display: grid;
  grid-template-columns: 240px 3fr;
}

.product-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)) ;
  gap: 2em;
  padding: 1em;
  margin: 0;
}

.filter-controls {
  text-align: left;
   padding: 1em 0 1em 1em;
}

.filter-controls > div {
  position: sticky;
  top:79px;
  padding: 1em;
  z-index: 9;
  background-color: white;
  border-radius: 4px;
  box-shadow: 0 0 9px -3px #c8c8c8;
 
}
.filter-controls input + label {margin-left: .5em;}

@media (max-width: 960px) {
  .grid > main,
  .filter-controls {grid-column: 1 / -1;}

  .filter-controls { padding: 1em 1em 0 1em;}

  .filter-controls > div {display: flex; position: relative;top: 0} 

  .filter-controls > div fieldset {display: flex; flex-wrap:wrap}
  .filter-controls > div  h3 {flex-basis:100%; margin: 0 0 .5em 0;}
  .filter-controls > div fieldset div {flex-grow:1}

}
</style>
