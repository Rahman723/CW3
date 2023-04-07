<template>
    <main>
        <div v-for="product in filteredProducts()" :key="product.id" class="productS">
            <h2>{{ product.title }}</h2>
            <figure>
                <img v-bind:src="product.image" />
            </figure>
            <p v-html="product.location"></p>
            <p>Price: {{ product.price }}</p>
            <p>Available stock: {{ product.space }}</p>
            <button @click="addProduct(product)" :disabled="product.space < 1">Add To Cart</button>
        </div>
    </main>
</template>
<script>
export default{
    name: "ProductList",
    props:["cart", "products", "filteredProducts"],
    methods:{
            addProduct(product){
                this.$emit('addProduct', product);
            }
        }
}
</script>