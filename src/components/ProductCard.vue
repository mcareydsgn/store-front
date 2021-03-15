<template>
  <li class="card" @click="showCard">
    <div class="product-image-container">
      <img :src="product.image" :alt="product.title">
    </div>
    <div class="info-box">
      <p class="price">{{product.price | prettyPrice}}</p>
    </div>
    <div class="favorite">&#10084;</div>
  </li>
</template>

<script>
export default {
  name: 'ProductCard',
  props: {
    product: {
      type: Object,
      requried: true
    }
  },
  methods:{
    showCard(){
      console.log(this.product.id)
      this.$emit('show-card', this.product.id)
    }
  },
  filters: {
    prettyPrice(price){
      return `$${price.toFixed(2)}`
    }
  }
}
</script>

<style scoped>
 .card {
   display: flex;
   flex-direction: column;
   padding: 1em;
    box-shadow: 0 0 9px -3px #c8c8c8;
    border-radius: 4px;
   background-color: white;
 }

 .card:hover {
   cursor: pointer;
 }

.product-image-container {margin-bottom: 1em;}
.product-image-container img { max-width: 150px; max-height: 150px;}

.card .info-box {margin-top: auto; }
.card .info-box .price {color:rgb(36, 211, 115); font-weight: bolder; font-size:1.4rem;} 

.card .favorite {position: absolute; color:rgb(228, 210, 213); z-index:5;}
.card .favorite:hover {color:rgb(241, 182, 212)} 
</style>
