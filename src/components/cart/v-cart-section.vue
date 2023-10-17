<template>
    <v-cart 
        :cart_data="CART"
        :size_data="size"
        :size_data_for_cart_item="size"
    />
</template>

<script>
    import { mapActions, mapGetters } from 'vuex';
    import vCart from '../cart/v-cart'
 
    export default{
        name: 'v-product-card',
        components: {
            vCart
        },
        props: {
            
        },
        data () {
            return {
                
            }
        },
        computed: {
            ...mapGetters([
                'PRODUCTS',
                'CART'
            ]),
            product() {
                let result = {}
                let vm = this;
                this.PRODUCTS.products.map(function (item) {
                    if (item.id == vm.$route.query.product) {
                        result = item;
                    }
                })
                return result;
            }
        },
        methods: {
            ...mapActions([
                'GET_PRODUCTS_FROM_API',
                'ADD_TO_CART'
            ]),
            addToCart() {
                this.ADD_TO_CART(this.product)
            }
        },
        mounted() {
            this.GET_PRODUCTS_FROM_API()
        },
    }
</script>