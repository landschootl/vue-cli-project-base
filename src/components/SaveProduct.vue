<template>
    <div id="save-product">
<!--        <md-dialog :md-active.sync="showDialog">-->
<!--            <md-dialog-title>Créer un produit</md-dialog-title>-->
<!--            <md-card-content>-->
<!--                <div class="md-layout md-gutter">-->
                    <h2>{{editMode ? `Modification du produit ${this.productUpdate.id}` : 'Créer un produit'}}</h2>
                    <div class="md-layout-item md-small-size-100">
                        <md-field>
                            <label for="first-name">Nom</label>
                            <md-input name="first-name" id="first-name" autocomplete="given-name" v-model="productUpdate.name" />
                        </md-field>
                    </div>
                    <div class="md-layout-item md-small-size-100">
                        <md-field>
                            <label for="last-name">Prix</label>
                            <span class="md-prefix">€</span>
                            <md-input name="last-name" id="last-name" autocomplete="family-name" v-model="productUpdate.price" />
                        </md-field>
                    </div>
<!--                </div>-->
<!--            </md-card-content>-->
<!--            <md-dialog-actions>-->
                <md-button class="md-primary" @click="saveProduct()">Enregistrer</md-button>
                <md-button class="md-danger" v-if="editMode" @click="deleteProduct()">Supprimer</md-button>
<!--            </md-dialog-actions>-->
<!--        </md-dialog>-->
    </div>
</template>

<script>
    export default {
        name: "SaveProduct",
        props: {
            product: Object,
            editMode: Boolean
        },
        data() {
            return {
                // showDialog: false
                productUpdate: {name: "default", price: 0}
            }
        },
        watch: {
            product() {
                console.log('change');
                this.productUpdate = Object.assign({}, this.product);
            }
        },
        methods: {
            saveProduct() {
                this.$emit('saveProduct', this.productUpdate);
                // this.showDialog = false;
            },
            deleteProduct() {
                this.$emit('deleteProduct', this.productUpdate);
                // this.showDialog = false;
            }
        }
    }
</script>

<style scoped>
 #save-product {
     margin: 10px;
 }
</style>
