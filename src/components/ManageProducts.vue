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
    <div v-else>
      <p>Nombre de produit : {{this.productsFiltered.length}}</p>
      <p>Prix des produits : {{totalPrice}}</p>
      <md-field>
        <label for="search">Recherche</label>
        <md-input type="text" name="search" v-model="search" />
      </md-field>
      <md-list>
        <Product v-for="(product, index) in productsFiltered" :key="'product-'+index" :product="product" @selectProduct="updateProduct($event)"/>
      </md-list>
      <md-button class="md-primary md-raised" @click="createProduct()">Créer un produit</md-button>
      <SaveProduct :product="newProduct" :edit-mode="newProduct.id != null" :active="saveProductActive" @saveProduct="saveProduct($event)" @deleteProduct="deleteProduct($event)" @close="saveProductActive=false"/>
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
        saveProductActive: false,
        newProduct: {},
        search: ''
      }
    },
    computed: {
      totalPrice() {
        return this.productsFiltered.length === 0
          ? 0
          : this.productsFiltered
                  .map(product => product.price)
                  .reduce((total, price) => total + price)
                  .toFixed(2);
      },
      productsFiltered() {
        return this.search === ''
                ? this.products
                : this.products.filter(product => product.name.includes(this.search));
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
                    this.saveProductActive = false;
                  })
                  .catch(e => {
                    console.log('error -> ', e);
                  });
        } else {
          this.axios.post(`${this.backendUrl}/products`, productUpdate)
                  .then(() => {
                    this.initProducts();
                    this.saveProductActive = false;
                  })
                  .catch(e => {
                    console.log('error -> ', e);
                  });
        }
      },
      updateProduct(product) {
        this.newProduct = product;
        this.saveProductActive = true;
      },
      createProduct() {
        this.newProduct = Object.assign({name: 'name', price: 0});
        this.saveProductActive = true;
      },
      deleteProduct(product) {
        this.axios.delete(`${this.backendUrl}/products/${product.id}`)
                .then(() => {
                  this.initProducts();
                  this.newProduct = {name: 'name', price: 0};
                  this.saveProductActive = false;
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
