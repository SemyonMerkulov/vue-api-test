<template>

  <div class="header__cart" @click="toggleDropdown">
    <span class="header__cart-ico">
      <svg xmlns="http://www.w3.org/2000/svg" fill="#485766" viewBox="0 0 512 512"><path d="M169.6 377.6c-22.882 0-41.6 18.718-41.6 41.601 0 22.882 18.718 41.6 41.6 41.6s41.601-18.718 41.601-41.6c-.001-22.884-18.72-41.601-41.601-41.601zM48 51.2v41.6h41.6l74.883 151.682-31.308 50.954c-3.118 5.2-5.2 12.482-5.2 19.765 0 27.85 19.025 41.6 44.825 41.6H416v-40H177.893c-3.118 0-5.2-2.082-5.2-5.2 0-1.036 2.207-5.2 2.207-5.2l20.782-32.8h154.954c15.601 0 29.128-8.317 36.4-21.836l74.882-128.8c1.237-2.461 2.082-6.246 2.082-10.399 0-11.446-9.364-19.765-20.8-19.765H135.364L115.6 51.2H48zm326.399 326.4c-22.882 0-41.6 18.718-41.6 41.601 0 22.882 18.718 41.6 41.6 41.6S416 442.082 416 419.2c0-22.883-18.719-41.6-41.601-41.6z"/></svg>
    </span>
    <span class="header__cart-amount">{{ data.amount }}</span> 
    <div class="header__cart-dropdown" v-if="showDropdown">
      <ul class="header__cart-list" v-for="item in data.list">
        <li class="header__cart-item">
          <div class="header__cart-col">
            <span class="header__cart-title">({{ item.amount }}) {{ item.product.title }}</span> 
            <span class="header__cart-price">{{ item.total }} Р</span>
          </div>
          <button class="header__cart-btn" @click="deleteCartItem(item.product.id)">
          </button> 
        </li>
      </ul>
      <p class="header__cart-total">Всего: <b>{{ data.price }} Р</b></p>
    </div>
  </div> 	
</template>

<script>

import axios from 'axios'	
const url = 'http://vue-tests.dev.creonit.ru/api/cart/';


export default {


  name: 'ProductCart',
  data: function() {
    return {
      showDropdown: false,
      data: ''
    }
  },
  props: {
    msg: String
  },
  methods: {
    toggleDropdown: function() {
      if (!this.showDropdown) {
        this.showDropdown = true;
      } 
      else {
        this.showDropdown = false;
      }
    },
  	fetchCartList: function(){
  		axios.get(url + 'list')
				.then(response => {
					console.log('cartList:');
          console.log(response);

          this.data = response.data;
				})
				.catch(error => {
				    console.log(error)
				 })
  	},
    deleteCartItem: function(id){
      axios.delete(url + 'product/' + id);
        // .then(response => {
        //   console.log(response);
        // })
        // .catch(error => {
        //     console.log(error)
        //  })
    }
  },
  created: function(){
    this.fetchCartList();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .header__cart {
    cursor: pointer;
    display: flex;
    align-items: flex-start;
    position: relative;
  }

  .header__cart-dropdown {
    background: #fff;
    border-radius: 0 0 5px 5px;
    box-shadow: 0 1px 3px 0 rgba(0,0,0,.12), 0 0 3px 0 rgba(0,0,0,.06);
    position: absolute;
    top: 100%;
    right: 0;
    margin-top: 12px;
    min-height: 50px;
    text-align: left;
    width: 220px;
  }
  
  .header__cart-amount {
    background: #485766;
    display: block;
    border-radius: 50%;
    color: #fff;
    font-size: 10px;
    font-weight: 700;
    line-height: 18px;
    height: 20px;
    width: 20px;
    text-align: center;
  }

  .header__cart-ico {
    display:  block;
    height: 28px;
    width: 28px;
  }

  .header__cart-btn {
    border: 1px solid #485766;
    border-radius: 50%;
    background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGZpbGw9IiM0ODU3NjYiIHZpZXdCb3g9IjAgMCA1MTIgNTEyIj48cGF0aCBkPSJNNDA1IDEzNi43OThMMzc1LjIwMiAxMDcgMjU2IDIyNi4yMDIgMTM2Ljc5OCAxMDcgMTA3IDEzNi43OTggMjI2LjIwMiAyNTYgMTA3IDM3NS4yMDIgMTM2Ljc5OCA0MDUgMjU2IDI4NS43OTggMzc1LjIwMiA0MDUgNDA1IDM3NS4yMDIgMjg1Ljc5OCAyNTZ6Ii8+PC9zdmc+) no-repeat;
    cursor: pointer;
    display: block;
    height: 16px;
    width: 16px;
    outline: none;
  }

  .header__cart-list {
    list-style: none;
    padding: 0 10px;
    margin: 0;
  }

  .header__cart-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 40px;
  }

  .header__cart-title {  
    font-size: 12px;
    display: inline-block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 130px;
  }

  .header__cart-price {
    font-weight: 700;
    font-size: 12px;
    margin-left: 5px;
  }

   .header__cart-total {
    border-top: 1px solid rgba(0,0,0,.06);
    font-size: 16px;
    text-align: right;
    margin: 0;
    padding: 8px 10px;
  }

</style>
