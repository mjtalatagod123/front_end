<template>
    <div>
        <NavBar />
        <div class="container">
            <h1>My Products</h1>
            <div class="row">
                <div class="col-6">
                    <h5>List of Products</h5>
                    <hr>
                    <ul class="list-group">
                        <li class="list-group-item btn-li"  v-for="product in products" :key="product.id" @click="onSelected(product)">
                            {{product.name}} <span class="float-right">{{product.price}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <ProductView :product="selectedProduct" @saved="onChange" @newProduct="onNew" @deleted="onChange" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            products: [],
            selectedProduct: {}
        }
    },
    methods: {
        async getMyProducts() {
            await this.$axios.get('/api/products')
            .then((res)=>{
                if(res.status==200) {
                    this.products = res.data
                }
            })
        },
        onChange() {
            this.getMyProducts()
            this.selectedProduct = {}
        },
        onSelected(product) {
            this.selectedProduct = product;
        },
        onNew() {
            this.selectedProduct = {}
        },
    },
    created() {
        this.getMyProducts()
    }
}
</script>

<style>
.row {
    padding-top: 30px;
    background-color: goldenrod;
}
.container {
    margin-top: 20px;
}
h1 {
    text-align: center;
}
.btn-li {
    cursor: pointer;
}
.btn-li:hover {
    background-color: antiquewhite;
}
</style>