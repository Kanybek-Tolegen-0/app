<template>
    <div class="product">
        <h1>{{product.title}}</h1>
        <p>Цена: {{product.price}}</p>
        <p>id: {{product.id}}</p>
    </div>
</template>

<script>
export default ({
    name: "Product",
    props: {
       product: Object,
    },
})
</script>

<style>
    .product {
        text-align: center;        
    }
</style>
