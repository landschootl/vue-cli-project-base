<template>
    <div id="save-product">
        <md-dialog :md-active="active">
            <md-dialog-title>{{editMode ? `Modification du produit ${this.productUpdate.id}` : 'Créer un produit'}}</md-dialog-title>
            <md-card-content>
                <div class="md-layout md-gutter">
                    <div class="md-layout-item md-small-size-100">
                        <md-field>
                            <label for="product-name">Nom</label>
                            <md-input name="product-name" id="product-name" v-model="productUpdate.name" />
                        </md-field>
                    </div>
                    <div class="md-layout-item md-small-size-100">
                        <md-field>
                            <label for="product-price">Prix</label>
                            <span class="md-prefix">€</span>
                            <md-input name="product-price" id="product-price" v-model="productUpdate.price" />
                        </md-field>
                    </div>
                </div>
            </md-card-content>
            <md-dialog-actions>
                <md-button class="md-primary" @click="saveProduct()">Enregistrer</md-button>
                <md-button class="md-danger" v-if="editMode" @click="deleteProduct()">Supprimer</md-button>
                <md-button class="md-danger" @click="close()">Fermer</md-button>
            </md-dialog-actions>
        </md-dialog>
    </div>
</template>

<script>
    export default {
        name: "SaveProduct",
        props: {
            product: Object,
            editMode: Boolean,
            active: Boolean
        },
        data() {
            return {
                productUpdate: {name: "default", price: 0}
            }
        },
        watch: {
            product() {
                this.productUpdate = Object.assign({}, this.product);
            }
        },
        methods: {
            saveProduct() {
                this.$emit('saveProduct', this.productUpdate);
            },
            deleteProduct() {
                this.$emit('deleteProduct', this.productUpdate);
            },
            close() {
                this.$emit('close');
            }
        }
    }
</script>

<style scoped>
 #save-product {
     margin: 10px;
 }
</style>
