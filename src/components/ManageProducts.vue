<template>
  <div class="hello">
    <h1>Liste de mes produits : </h1>
    <ul>
      <p v-if="!productsIsLoad">Communes en chargement ...</p>
      <Product v-for="(product, index) in products" :key="'product-'+index" :product="product"/>
      <SaveProduct :product="newProduct" @saveProduct="saveProduct($event)"/>
    </ul>
  </div>
</template>

<script>
  import Product from "./Product";
  import SaveProduct from "./SaveProduct";

  export default {
    name: 'ManageProducts',
    components: {
      Product,
      SaveProduct
    },
    data() {
      return {
        backendUrl: 'https://node-baseapi.herokuapp.com/api',
        productsIsLoad: false,
        products: [],
        newProduct: {name: 'nameDefault', price: 0}
      }
    },
    created() {
      this.initProducts();
    },
    methods: {
      initProducts() {
        this.axios.get(`${this.backendUrl}/products`)
                .then(response => {
                  this.products = response.data.result;
                  this.productsIsLoad = true;
                })
                .catch(e => {
                  console.log('error -> ', e);
                });
      },
      saveProduct() {
        this.axios.post(`${this.backendUrl}/products`, this.newProduct)
                .then(() => {
                  // this.products.push(response.data.result);
                  this.initProducts();
                })
                .catch(e => {
                  console.log('error -> ', e);
                });
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
