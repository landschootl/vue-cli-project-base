<template>
    <div>
        <h2>Creation d'un produit</h2>
        <p>Nom du produit : <input v-model="product.name" type="text"/></p>
        <p>Prix du produit : <input v-model.number="product.price" type="number"/></p>
        <button @click="saveProduct()">Enregistrer</button>
    </div>
</template>

<script>
    export default {
        name: "SaveProduct",
        props: {
            product: Object
        },
        methods: {
            saveProduct() {
                this.$emit('saveProduct', this.product);
            }
        }
    }
</script>

<style scoped>

</style>
