<template>
    <div>
        <button class="btn btn-success float-right" @click="onNew">
            New Product
        </button>
        <h5>Product View</h5>
        <hr>

        <form action="" @submit.prevent="onSave">
            <b-form-group label="Name">
                <b-form-input v-model="product.name"></b-form-input>
            </b-form-group>
            <b-form-group label="Description">
                <b-form-input v-model="product.description"></b-form-input>
            </b-form-group>
            <b-form-group label="Quantity">
                <b-form-input v-model="product.quantity"></b-form-input>
            </b-form-group>
            <b-form-group label="Price">
                <b-form-input v-model="product.price"></b-form-input>
            </b-form-group>
            <b-form-group label="Manufactured On">
                <b-form-input type="date" v-model="product.manufactured_on"></b-form-input>
            </b-form-group>
            <b-form-group label="Category">
                <b-form-select v-model="product.category" :options="options"></b-form-select>
            </b-form-group>
            <b-form-group>
                <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger float-right" type="button" @click="onDelete" v-if="product.id">Delete Product</button>
            </b-form-group>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        product: {},
    },
    data() {
        return{
            options: [
                { value: 'food', text: 'Food' },
                { value: 'beverage', text: 'Beverage' },
                { value: 'cleaning', text: 'Cleaning' },
                { value: 'personal use', text: 'Personal use'}
            ]
        }
    },
    methods: {
        async onSave() {
            try {
                if(!this.product.id) {
                    await this.$axios.post('/api/products', this.product)
                }else{
                    await this.$axios.put('/api/products/' + this.product.id, this.product)
                }

                this.$emit('saved')
            } catch (err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newProduct')
        },
        async onDelete() {
            try {
                this.$axios.delete('/api/products/' + this.product.id)
                this.$emit('deleted')
            } catch (err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>