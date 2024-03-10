<script setup lang="ts">
    import { ref, computed } from 'vue';
    import { type Product, getProducts } from "@/model/products";
    const products = ref([] as Product[]);

    type CartItem = {
        product: Product;
        quantity: number;
    }
    const cart = ref([] as CartItem[])
    
    function addToCart(product: Product)
    {
        const item = cart.value.find((item) => item.product.id === product.id);
        if(item)
        {
            item.quantity++;
        }
        else
        {
            cart.value.push({ product, quantity: 1});
        }
    }

    const addToCart2 = (product: Product) => {
        const item = cart.value.find((item) => item.product.id === product.id);
        if(item)
        {
            item.quantity++;
        }
        else
        {
            cart.value.push({ product, quantity: 1});
        }
    }

    products.value = getProducts();

    const total = computed(() => cart.value.reduce((total, item) => total + item.product.price * item.quantity, 0))
</script>

<template>
    <div v-for="product in products" :key=" product.id" class="products-list">
        <div class="card">
  <div class="card-image">
    <img :src="product.thumbnail" :key="product.title">
  </div>
  <div class="card-content">
    <h3>{{ product.title }}</h3>
    <i>{{ product.brand }}</i>
    <p class="price"> {{ product.price }}</p>
    <p> {{ product.description }}</p>
    <button @click="addToCart(product)" class="button is-primary">Add to Cart</button>
</div>
    </div>
    </div>
    <div class="flyout">
        <h1 class="title">
            The Cart
        </h1>
        <ul class="cart">
            <li v-for="item in cart" :key="item.product.id">
                <img :src="item.product.thumbnail" :alt="item.product.title" >
                {{ item.product.title }} x {{ item.quantity }} = ${{ item.product.price * item.quantity }}
            </li>
        </ul>
        {{ cart.length }} items totalling ${{ total }}
    </div>
</template>

<style scoped>
    .product-list {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
    }
    .card
    {
        flex-basis: 15rem;
        flex-grow: 1;
        margin: .5rem;

    }
    h3 {
        font-weight: bold;
    }
    .price{
        font-weight: bold;
        font-size: xx-large;
        color: hotpink;
        float: right;
    }
    .flyout
    {
        position: fixed;
        top: 0;
        right: 0;
        width: 20rem;
        height: 100%;
        background-color: white;
        box-shadow: -1px 0 5px 0px (0,0,0,5);
        border: 1px solid #ccc;
        z-index: 100;
        transform: translateX(100%);
        transition: transform 1s ease-in-out;
        padding: 1rem;
    }
    .flyout.open, flyout:hover
    {
        transform: translateX(0);
    }
    
    .cart li
    {
        display: flex;
        height: 1rem;
        margin-bottom: 1rem;
    }
</style>