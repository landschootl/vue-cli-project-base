<template>
    <md-list-item class="item-btn" @click="selectProduct()">{{product.name}} <code>{{product.price}}</code></md-list-item>
</template>

<script>
    export default {
        name: "Product",
        props: {
            product: Object
        },
        methods: {
            selectProduct() {
                this.$emit('selectProduct', this.product);
            }
        }
    }
</script>

<style scoped>
    .item-btn {
        cursor: pointer;
    }
</style>
