<template style="font-family: sans-serif; background-color: #fffdd0;">
  <!-- <div id="app">
    <header>
    <h1>{{ sitename }}</h1>
    <button @click="showCheckout">{{ this.cart.length }} Checkout </button>
  </header>
  <main>
    <product-list @addProduct='addToCart'></product-list>
    <checkout :cart.sync='cart'></checkout>
  </main>
  </div> -->
  <div id="app">
    <header>
    <h1>{{ sitename }}</h1>
    <button @click="showCheckout">{{ this.cart.length }} Checkout </button>
  </header>
  <div  v-if="showProduct">
                    <div><!--Choosing the order of displayed items(ascending or descending)-->
                        <div>
                            <input class="form-check-input" value="Ascending" type="radio" id="ascending"
                                v-model="lowHigh" />
                            <label class="form-check-label" for="ascending">Ascending</label>
                        </div>
                        <div>
                            <input class="form-check-input" value="Descending" type="radio" id="descending"
                                v-model="lowHigh" />
                            <label class="form-check-label" for="descending">Descending</label>
                        </div>
                    </div>
                    <form>
                        <div>
                            <div>
                                <div><!--Search input-->
                                    <input type="text" v-model="searchInput"
                                        placeholder="Search Course">
                                </div>
                            </div>
                            <div>
                                <div><!--div for choosing the sorting method-->
                                    <select name="sortBy" v-model="sortBy">
                                        <option>--Sort By--</option>
                                        <option value="price">Price</option>
                                        <option value="location">Location</option>
                                        <option value="space">Avalibility</option>
                                        <option value="title">Subject</option>
                                    </select>
                                </div>
                            </div>

                        </div>
                    </form>
                    <!--Displaying all the courses-->
                    <product-list :cart="cart" :products="products" :filteredProducts="filteredProducts" @addProduct='addToCart'></product-list>
                </div>
                <div v-else>
                  <checkout :cart="cart" :products="products" @removeFromCart="removeFromCart" @submitForm="submitForm"></checkout>
                </div>
                </div>
</template>

<script>

import productList from './components/ProductList.vue';
import checkout from './components/FormList.vue';
export default{
  name: 'App',
  components:{
    productList,
    checkout
  },
  data(){
    return {
      sitename: 'Vue.js Pet Depot', 
      cart: [],
      showProduct: true,
      lowHigh: 'Ascending',
      searchInput: '',
      sortBy: '--Sort By--',
      order:{
        firstName: '',
        lastName: '',
        phone: ''
      },
      products: [],
    }
  },
  methods: {
    showCheckout(){
      this.showProduct=this.showProduct ? false : true;
    },
    addToCart(product){
      //console.log("addProduct event received by root component.");
      this.cart.push(product);
      product.space -= 1;
      console.log(this.cart);
    },
    checkOut() {//checkout page
                        let show = this.cart
                        return show
    },
                    submitForm(){//placing order function
                        alert("ORDER SUBMITTED !!!");
                    },
                    sortByPrice: function (priceArray) {
                    function compare(a, b) {
                        if (a.price > b.price)
                            return 1;
                        if (a.price < b.price)
                            return -1;
                        return 0;
                    }
                    return priceArray.sort(compare);
                },
                sortByTitle: function (subjectArray) {
                    function compare(a, b) {
                        if (a.title > b.title)
                            return 1;
                        if (a.title < b.title)
                            return -1;
                        return 0;
                    }
                    return subjectArray.sort(compare);
                },
                sortByLocation: function (locationArray) {
                    function compare(a, b) {
                        if (a.location > b.location)
                            return 1;
                        if (a.location < b.location)
                            return -1;
                        return 0;
                    }
                    return locationArray.sort(compare);
                },
                sortByAva: function (avaArray) {
                    function compare(a, b) {
                        if (a.space > b.space)
                            return 1;
                        if (a.space < b.space)
                            return -1;
                        return 0;
                    }
                    return avaArray.sort(compare);
                },
                    filterProducts: function () { //sorting by price, Title, space, and location 
                    let tempProducts = this.products

                    tempProducts = tempProducts.filter((product) => {
                        return product.title.toLowerCase().match(this.searchInput.toLowerCase()) || product.location.toLowerCase().match(this.searchInput.toLowerCase())
                    })
                    if (this.sortBy == 'price') {
                        tempProducts = this.sortByPrice(tempProducts)
                    }
                    else if (this.sortBy == 'title') {
                        tempProducts = this.sortByTitle(tempProducts)
                    }
                    else if (this.sortBy == 'location') {
                        tempProducts = this.sortByLocation(tempProducts)
                    }
                    else if (this.sortBy == 'space') {
                        tempProducts = this.sortByAva(tempProducts)
                    }

                    if (this.lowHigh == 'Ascending') {
                        return tempProducts
                    }
                    else if (this.lowHigh == 'Descending') {
                        return tempProducts.reverse()
                    }
                    return tempProducts
                },
                cartCount(id){ //counts quantity of items in the cart
                    let count = 0
                    for (let i = 0; i < this.cart.length;i++){
                        if(this.cart[i].id === id){
                            count++;
                        }
                    }
                    return count;
                },
                removeDuplicates(arr) { //clears the check out info by removing the same course that is duplicated
                    var unique = [];
                    arr.forEach(element => {
                        if (!unique.includes(element)) {
                            unique.push(element);
                        }
                    });
                    return unique;
                },
                removeFromCart(id) { //removes items from the cart
                    let showCart = this.cart
                    let less = this.products
                    let count = 0
                    for (let i = 0; i < showCart.length; i++) {
                        //console.log(showCart[i].id)
                        if(count==0){
                        if (id == showCart[i].id) {
                            showCart.splice(i, 1)
                            count++;
                        }}
                    }
                 
                    for (let i = 0; i < less.length; i++) {
                        console.log(less[i].id)
                        if (id == less[i].id) {
                            less[i].space+= 1
                        }
                    } 
                  
                },
                filteredProducts(){
            //sorting by price, Title, Space, and location
            let tempProducts = this.products;

            tempProducts = tempProducts.filter((product) => {
              return (
                product.title
                  .toLowerCase()
                  .match(this.searchInput.toLowerCase()) ||
                product.location
                  .toLowerCase()
                  .match(this.searchInput.toLowerCase())
              );
            });
            if (this.sortBy == "price") {
              tempProducts = this.sortByPrice(tempProducts);
            } else if (this.sortBy == "title") {
              tempProducts = this.sortByTitle(tempProducts);
            } else if (this.sortBy == "location") {
              tempProducts = this.sortByLocation(tempProducts);
            } else if (this.sortBy == "space") {
              tempProducts = this.sortByAva(tempProducts);
            }

            if (this.lowHigh == "Ascending") {
              return tempProducts;
            } else if (this.lowHigh == "Descending") {
              return tempProducts.reverse();
            }
            return tempProducts;
          },

  },
  created: function(){
    const store=this;
        fetch('http://localhost:3000/collection/lessons').then(
            function(response){
                response.json().then(
                    function(json){
                        store.products=json;
                    }
                );
            }
        );}
}

</script>
<style>

#app{
  font-family: Avenir, Arial, Helvetica, sans-serif;
  background-color: #fffdd0;
}
.productS{
  display: inline-block;
              margin: 10px;
              width: 310px;
              height: 420px;
              border: 1px solid black;
              border-radius: 10%;
              padding: 10px;
              background-color: #617c58;
}
img{
  width: 150px; height: 160px; display: inline-block
}
</style>