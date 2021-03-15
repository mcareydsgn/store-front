<template>
    <div class="product-modal" >
        <div class="modal-body">
            <div>
                <img :src="product.image" alt="showDetails.title">
            </div>
            <div class="info-box">
                <p class="title">{{product.title}}</p>
                <p class="description">{{product.description}}</p>
                <p class="price">{{product.price | prettyPrice}}</p>
                <button class="button">
                    Add To Cart
                </button>
            </div>
            <div class="favorite">&#10084;</div>
        </div>
        <div class="modal-backdrop" @click="close()">

        </div>
    </div>
</template>

<script>
export default {
  name: 'ProductModal',
  props: {
    product: {
      type: Object,
      requried: true
    }
  },
  methods:{
      close(){
          this.$emit('close')
      },
  },
  filters: {
    prettyPrice(price){
      return `$${price.toFixed(2)}`
    }
  }
}
</script>

<style scoped>
.product-modal {
    position: fixed;
    top: 0;
    width: 100%;
    left: 0;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
}


.product-modal .modal-body {
    position: relative;
    padding: 2vw;
    width: 80vw;
    z-index: 10;
    background-color: white;
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    max-width: 700px;
}

.product-modal .modal-body > div {

    flex-basis: 250px;
    padding: 1em;
    flex-grow: 1;
}

.product-modal .modal-body img { max-width: 300px; max-height: 300px;}
.modal-backdrop {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    background-color: rgb(20 255 212 / 82%);
    height: 100%;
}

.product-modal .modal-body .info-box {text-align: left;} 
.product-modal .modal-body .info-box p {margin-bottom: .5em;} 
.product-modal .modal-body .info-box .title {font-weight: bolder; font-size:1.2rem;} 
.product-modal .modal-body .info-box .price {color:rgb(36, 211, 115); font-weight: bolder; font-size:2rem;} 
.product-modal .modal-body .info-box .button {background-color:rgb(0 147 218); color:white; font-weight: bolder; padding:.5rem; border:none; border-radius:3px; font-size: 1rem;} 

.product-modal .modal-body .favorite {position: absolute; color:rgb(228, 210, 213); z-index:5; top:0; cursor: pointer;}
.product-modal .modal-body .favorite:hover {color:rgb(241, 182, 212)} 
</style>
