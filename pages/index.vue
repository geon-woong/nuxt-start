<template>
      <div class="app">
        <main>
        <SearchInput v-model="searchKeyword" @search="searchProducts"></SearchInput>
          <ul>
            <li v-for="product in products"  :key="product.id" class="item flex" @click="moveTo(product.id)">
              <img class="product-image" :src="product.imageUrl" :alt="product.name">
              <p>{{product.name}}</p>
              <span>{{product.price}}</span>
            </li>
          </ul>
        </main>
        <div class="cart-wrapper">
          <button class="btn" @click="moveToCart">장바구니 바로가기</button>
        </div>
      </div>
      <!-- <ProductList></ProductList> -->
</template>

<script>
import axios from 'axios'
import SearchInput from '../components/SearchInput.vue';
import { fetchProductByKeyword } from '~/api'

// import ProductList from '../components/ProductList.vue'

export default {
    components: { SearchInput },
    // components: { ProductList },
    async asyncData() {
        const response = await axios.get("http://localhost:3000/products");
        // 같은 url로 사진을 불러오기 떄문에 아래의 작업을 수행
        const products = response.data.map((item) => ({
            ...item,
            imageUrl: `${item.imageUrl}?ramdom=${Math.random()}`,
        }));
        return { products };
    },
    data() {
      return {
        searchKeyword: '',
      }
    },
    methods: {
        moveTo(id) {
            this.$router.push(`detail/${id}`);
        },
        async searchProducts(){
          const response =  await fetchProductByKeyword(this.searchKeyword)
          this.products = response.data.map((item) => ({
            ...item,
            imageUrl: `${item.imageUrl}?ramdom=${Math.random()}`,
          }));
        },
        moveToCart(){
          this.$router.push('/cart')
        }
      }
}
</script>


<style scoped>
.flex {
  display: flex;
  justify-content: center;
}
.item {
  display: inline-block;
  width: 400px;
  height: 300px;
  text-align: center;
  margin: 0 0.5rem;
  cursor: pointer;
}
.product-image {
  width: 400px;
  height: 250px;
}
.app {
  position: relative;
}
.cart-wrapper {
  position: sticky;
  float: right;
  bottom: 50px;
  right: 50px;
}
.cart-wrapper .btn {
  display: inline-block;
  height: 40px;
  padding: 10px;
  font-size: 1rem;
  font-weight: 500;
  border: none;border-radius: 10px;
  background-color: darkolivegreen;color: #fff;
}
</style>