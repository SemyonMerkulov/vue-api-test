<template>
  <div id="app">
    <header class="header">
      <div class="container header__container">
        <h1 class="logo">Шины&Диски</h1>
        <ProductCart ref="ProductCart" />
      </div>
      <div class="header__nav">
        <nav class="nav">
          <div class="container">
            <a href="#" 
              :class="{
                nav__link: true,
                nav__link_active : activeNavEl == 0
              }"
              @click.prevent="selectCategory('shiny'), activateNavEl(0)">
              Шины
            </a>
            <a href="#"
              :class="{
                nav__link: true,
                nav__link_active : activeNavEl == 1
              }" 
              @click.prevent="selectCategory('diski'), activateNavEl(1)">
              Диски
            </a>
          </div>
        </nav>
      </div>
    </header>
    <div class="container">
      <ProductList ref="ProductList" @addProduct="addToCart"/>
    </div>
  </div>
</template>

<script>
import ProductList from './components/ProductList.vue'
import ProductCart from './components/ProductCart.vue'
import axios from 'axios' 

const url = 'http://vue-tests.dev.creonit.ru/api/cart/product/';

export default {
  name: 'app',
  components: {
    ProductList,
    ProductCart
  },
  data: function() {
    return {
      activeNavEl: 0
    }
  },
  methods: {
    selectCategory: function(slug) {
      this.$refs.ProductList.fetchList(slug);
    },
    activateNavEl: function(el) {
      this.activeNavEl = el;
    },
    addToCart: function(id) {
      axios.post(url + id);
      this.$refs.ProductCart.fetchCartList();  
    }
  }
}
</script>

<style>
#app {
  font-family: 'Open Sans', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-top: 90px;
}

.container {
  margin: auto;
  width: 980px;
}

.logo {
  font-size: 26px;
  margin: 0;
}

.btn {
  background: #fff;
  border: 2px solid #485766;
  border-radius: 30px;
  color: #485766;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  outline: none;
  padding: 8px 25px;
}

.btn:hover,
.btn:focus {
  background: #485766;
  color: #fff;
}

.header {
  background: #fff;
  box-shadow: 0 1px 3px 0 rgba(0,0,0,.12), 0 0 3px 0 rgba(0,0,0,.06);
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
}

.header__container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 50px;
}

.nav {
  text-align: left;
  padding: 10px 0;
}

.nav__link {
  display: inline-block;
  color: #2c3e50;
  font-weight: 400;
  margin-right: 10px;
  text-decoration: none;
}

.nav__link:hover,
.nav__link:focus {
  text-decoration: underline;
}

.nav__link_active {
  font-weight: 700;
}

</style>
