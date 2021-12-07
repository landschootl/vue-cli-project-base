<template>
  <div>
    <img src="https://cdn-icons-png.flaticon.com/512/2038/2038854.png" id="img-store"/>
    <md-toolbar class="md-primary">
      <h3 class="md-title">Liste de mes produits :</h3>
    </md-toolbar>
    <div v-if="!productsIsLoad">
      <p>Produits en chargement ...</p>
      <md-progress-spinner md-mode="indeterminate"></md-progress-spinner>
    </div>
    <div>
      <p>Nombre de produit : {{this.products.length}}</p>
      <p>Prix des produits : {{totalPrice}}</p>
      <md-list>
        <Product v-for="(product, index) in products" :key="'product-'+index" :product="product" @selectProduct="updateProduct($event)"/>
      </md-list>
      <md-button class="md-primary md-raised" @click="createProduct()">Créer un produit</md-button>
      <SaveProduct :product="newProduct" :edit-mode="newProduct.id != null" @saveProduct="saveProduct($event)" @deleteProduct="deleteProduct($event)"/>
    </div>
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
    computed: {
      totalPrice() {
        return this.products.length === 0
          ? 0
          : this.products
                  .map(product => product.price)
                  .reduce((total, price) => total + price)
                  .toFixed(2);
      }
    },
    created() {
      setTimeout(this.initProducts, 3000);
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
      saveProduct(productUpdate) {
        if(productUpdate.id != null) {
          this.axios.put(`${this.backendUrl}/products/${productUpdate.id}`, productUpdate)
                  .then(() => {
                    this.initProducts();
                  })
                  .catch(e => {
                    console.log('error -> ', e);
                  });
        } else {
          this.axios.post(`${this.backendUrl}/products`, productUpdate)
                  .then(() => {
                    this.initProducts();
                  })
                  .catch(e => {
                    console.log('error -> ', e);
                  });
        }
      },
      updateProduct(product) {
        this.newProduct = product;
      },
      createProduct() {
        this.newProduct = Object.assign({name: 'nameDefault', price: 0});
      },
      deleteProduct(product) {
        this.axios.delete(`${this.backendUrl}/products/${product.id}`)
                .then(() => {
                  this.initProducts();
                  this.newProduct = {name: 'nameDefault', price: 0};
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
  #img-store {
    text-align: center;
    height: 100px;
    margin: 4px;
  }
  .md-progress-spinner {
    margin: 24px;
  }
</style>
