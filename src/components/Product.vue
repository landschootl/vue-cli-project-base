<template>
    <div>{{product.id}} - {{product.name}} - {{product.price}}</div>
</template>

<script>
    export default {
        name: "Product",
        props: {
            product: Object
        }
    }
</script>

<style scoped>

</style>
