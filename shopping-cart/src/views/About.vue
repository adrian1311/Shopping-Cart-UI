<template>
    <div class="about">
        <div class="row justify-content-center mt-2">
            <div class="col-sm-2 text-center">

            </div>
            <div class="col-sm-2 text-center">
                <input type="text" disabled class="form-control font-weight-bold" v-model="name">
            </div>
            <div class="col-sm-2 text-center">
                <input type="text" disabled class="form-control font-weight-bold" v-model="amount">
            </div>
            <div class="col-sm-2 text-center">
                <input type="text" disabled class="form-control font-weight-bold" v-model="price">
            </div>
            <div class="col-sm-2 text-center">
                <input type="text" disabled class="form-control font-weight-bold" v-model="forCart">
            </div>
            <div class="col-sm-2 text-center">

            </div>
        </div>
        <div class="row justify-content-center mt-2" v-for="(product,index) in this.productsInShop" :key="product.productName">
            <div class="col-sm-2 text-center">
                <label class="mt-2 text-align: right" style="text-align: right">{{index+1}}</label>
            </div>
            <div class="col-sm-2 text-center">
                <input type="text" disabled class="form-control" v-model="product.productName">
            </div>
            <div class="col-sm-2 text-center">
                <input type="text" disabled class="form-control" v-model="product.productAmount">
            </div>
            <div class="col-sm-2 text-center">
                <input type="text" disabled class="form-control" v-model="product.productPrice">
            </div>
            <div class="col-sm-2 text-center">
                <input type="number" class="form-control" max="10" v-model="product.orderedProducts">
            </div>
            <div class="col-sm-2 text-center">
                <button type="button" v-on:click="sendProduct(product)" class="btn btn-primary"><img src="../assets/addCart.png" width="25px" height="25px"  style="margin-right: 7px;">Add to cart</button>
            </div>
        </div>
        <div class="row justify-content-center mt-3">
            <div class="col-sm-10 border border-info">
                <div class="row justify-content-center mt-2">
                    <label class="font-weight-bold">PRODUCTS IN SHOPPING CART</label>
                </div>
                <div class="row justify-content-center mt-2">
                    <div class="col-sm-2 text-center">
                        <input type="text" disabled class="form-control input-sm control-label font-weight-bold" v-model="name">
                    </div>
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm control-label font-weight-bold" v-model="price">
                    </div>
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm font-weight-bold" v-model="forCart">
                    </div>
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm font-weight-bold" v-model="total">
                    </div>
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm font-weight-bold" v-model="discount">
                    </div>
                    <div class="col-sm-1 text-center">
                        <!--input type="text" disabled class="form-control input-sm" v-model="actions"-->
                    </div>
                </div>
                <div class="row justify-content-center mt-2 mb-2" v-for="productCart in this.shoppingCart" :key="productCart.productName">
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm " v-model="productCart.productName">
                    </div>
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm" v-model="productCart.productPrice">
                    </div>
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm" v-model="productCart.orderedProducts">
                    </div>
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm" v-model="productCart.totalPrice">
                    </div>
                    <div class="col-sm-2 text-xs-center">
                        <input type="text" disabled class="form-control input-sm" v-model="productCart.discount">
                    </div>
                    <div class="col-sm-1 text-xs-center">
                        <button type="button" class="btn btn-danger" v-on:click="deleteProduct(productCart)"><img src="../assets/clearCart.png" width="25px" height="25px"  style="margin-right: 7px;">Delete</button>
                    </div>
                </div>
                <div class="row justify-content-center mb-2 mt-2">
                <div class="col-sm-3 text-center">
                    <label class="font-weight-bold">TOTAL PRICE IN CART</label>
                </div>
                <div class="col-sm-3 text-center">
                    <input type="text" disabled class="form-control input-sm " v-model="totalWithOutDiscount">
                </div>
            </div>
                <div class="row justify-content-center mb-2 mt-2">
                <div class="col-sm-3 text-center">
                    <label class="font-weight-bold">DISCOUNT</label>
                </div>
                <div class="col-sm-3 text-center">
                    <input type="text" disabled class="form-control input-sm" v-model="priceDiscount">
                </div>
            </div>
                <div class="row justify-content-center mb-2 mt-2">
                    <div class="col-sm-3 text-center">
                        <label class="font-weight-bold">PRICE BEFORE 10% OF TOTAL WHEN > 100</label>
                    </div>
                    <div class="col-sm-3 text-center">
                        <input type="text" disabled class="form-control input-sm" v-model="discountWithOut100">
                    </div>
                </div>
                <div class="row justify-content-center mb-2 mt-2">
                    <div class="col-sm-3 text-center">
                        <label class="font-weight-bold">FINAL PRICE</label>
                    </div>
                    <div class="col-sm-3 text-center">
                        <input type="text" disabled class="form-control input-sm" v-model="totalPrice">
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
    // @ is an alias to /src
    const axios = require('axios').default;
    export default {
        name: 'About',
        components: {},
        data() {
            return {
                productsInShop: [],
                name: 'NAME',
                price: 'PRICE',
                amount: 'AMOUNT',
                forCart: 'AMOUNT IN CART',
                discount: 'PRICE WITH DISCOUNT',
                actions: 'ACTIONS',
                total: 'TOTAL PRICE',
                totalPrice:0,
                totalWithOutDiscount:0,
                priceDiscount:0,
                discountWithOut100: 0,
              shoppingCart: [],
            }

        },
        mounted() {
            this.sendInformation();
        },
        methods: {
            sendInformation() {
                let self = this;
                const url = "http://localhost:8080/shoppingCart/showProducts";
                axios.post(url)
                    .then(function (response) {
                        self.productsInShop = response.data;
                        console.log(self.productsInShop)
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            sendProduct(shopProduct) {
                let self=this;
                const url = "http://localhost:8080/shoppingCart/addProduct";
                axios.post(url, {
                    productName: shopProduct.productName,
                    productAmount: shopProduct.productAmount,
                    productPrice: shopProduct.productPrice,
                    totalPrice: shopProduct.totalPrice,
                    discount: shopProduct.discount,
                    orderedProducts: shopProduct.orderedProducts
                })
                    .then(function (response) {
                      self.shoppingCart = response.data.productsInCart;
                        self.totalPrice = response.data.totalPrice;
                        self.totalWithOutDiscount = response.data.totalPriceWithOutDiscount;
                        self.priceDiscount=response.data.totalDiscount;
                        self.discountWithOut100= response.data.discountWithOut100;
                        console.log(response)
                    })
                    .catch(function (error) {
                        console.log(error);
                    });

            },
            deleteProduct(shopProduct) {
                let self=this;
                const url = "http://localhost:8080/shoppingCart/deleteProduct";
                axios.post(url, {
                    productName: shopProduct.productName,
                    productAmount: shopProduct.productAmount,
                    productPrice: shopProduct.productPrice,
                    totalPrice: shopProduct.totalPrice,
                    discount: shopProduct.discount,
                    orderedProducts: shopProduct.orderedProducts
                })
                    .then(function (response) {
                        self.shoppingCart = response.data.productsInCart;
                        self.totalPrice = response.data.totalPrice;
                        self.totalWithOutDiscount = response.data.totalPriceWithOutDiscount;
                        self.priceDiscount=response.data.totalDiscount;
                        self.discountWithOut100= response.data.discountWithOut100;
                        console.log(response)
                    })
                    .catch(function (error) {
                        console.log(error);
                    });

            },
            createCart() {
                let self = this;
                const url = "http://localhost:8080/shoppingCart/addProductInCart";
                axios.post(url)
                    .then(function (response) {
                        self.productsInShop = response.data;
                        console.log(self.productsInShop)

                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            }
        }
    }
</script>