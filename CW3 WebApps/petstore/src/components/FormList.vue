<template>
    <div>
        <h2>Checkout</h2>
        <h3>Added products</h3>
        <div v-for='(product,index) in  removeDuplicates(cart)' :key="index" style="display: inline-block;
              margin: 10px;
              width: 410px;
              height: 220px;
              border: 1px solid black;
              border-radius: 10%;
              padding: 10px;
              background-color: #617c58;">
            <div style="width: 150px; height: 160px; display: inline-block">
              <h2 v-text="product.title"></h2>
              <p v-html="product.location"></p>
              <p>Price: {{product.price}}</p>
              <p>Q-ty: {{cartCount(product.id)}}</p>
              <button @click="removeFromCart(product.id)">Remove</button>
            </div>
            <div style="width: 150px; height: 160px; display: inline-block">
              <figure>
                <img v-bind:src="product.image" width="100%" />
              </figure>
            </div>
          </div>
        <p>
            <strong>Name</strong><input v-model="name" />
            <strong>Number</strong><input type="number" v-model="address" />
            <button @click="submitForm()">Submit</button>
        </p>
    </div>
</template>
<script>
export default{
    name: "FormList",
    props: ["cart", "products"],
    data(){
        return{
            name: "",
            address: "",
        }
    },
    methods:{
        submitForm(){
                this.$emit('submitForm');
            },
            removeFromCart(id){
                this.$emit('removeFromCart', id);
            },
            cartCount(id) {
            //counts space of items in the cart
            let count = 0;
            for (let i = 0; i < this.cart.length; i++) {
              if (this.cart[i].id === id) {
                count++;
              }
            }
            return count;
          },removeDuplicates(arr) {
            //clears the check out info by removing the same course that is duplicated
            var unique = [];
            arr.forEach((element) => {
              if (!unique.includes(element)) {
                unique.push(element);
              }
            });

            return unique;
          }
    }
}
</script>