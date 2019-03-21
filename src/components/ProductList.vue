<template>
	<section>
	  <div class="product__item" v-for="item in items">
	  	<h3 class="product__title">{{ item.title }}</h3>
	  	<div class="product__content">
	  		<div class="product__col">
	  			<a :href="item.image" class="product__preview-link">
	  				<img :src="item.image_preview" alt="" class="product__preview-img">
	  			</a>
	  		</div>
	  		<div class="product__col">
	  			<p class="product__price">Цена: <b>{{ item.price }} Р</b></p>
	  			<button class="btn" @click="addProduct(item.id)">В корзину</button>	
	  		</div>		
	  	</div>
	  </div>
  </section>
</template>

<script>
import axios from 'axios'	
const url = 'http://vue-tests.dev.creonit.ru/api/catalog/';


export default {


  name: 'ProductList',
  data: function() {
		return {
			items: []
		}
	},
  methods: {
  	fetchList: function(slug){
  		axios.get(url + slug)
				.then(response => {
					console.log(response)
					this.items = response.data.items
				})
				.catch(error => {
				    console.log(error)
				 })
  	},
  	addProduct: function(id) {
  		this.$emit('addProduct', id	);
  	}
  },
  created: function(){
  	this.fetchList('shiny');
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
	.product__content {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
	}

	.product__item {
		border-bottom: 1px solid rgba(0,0,0,.06);
		padding: 0 0 20px;
	}

	.product__title {
		text-align: left;
	}

	.product__preview-link {
		display: block;
		height: auto;
		width: 160px;
	}

	.product__preview-img {
		display: block;
		width: 100%;
	}
</style>
