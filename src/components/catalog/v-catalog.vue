<template>
    <div class="v-catalog">
        <div class="container">
            <div class="catalog__item">
                <div class="catalog__carts">
                    <div>
                        <form class="catalog__carts__tabs" action="">
                            <div @click="toggleTabStockAndFilterTrue('stock')">
                                <input checked hidden class="tab-input" id="stock" type="radio" name="tab">
                                <label for="stock" class="tab active">
                                    В наличии
                                </label>
                            </div>
                            <div @click="toggleTabStockAndFilter('order')">
                                <input hidden class="tab-input" id="order"  type="radio" name="tab">
                                <label for="order" class="tab">
                                    Под заказ
                                </label>
                            </div>
                        </form>
                    </div>
                    <div v-if="sortedProducts.length > 0">
                        <div v-if="tabStock" class="catalog__carts__produkcts">
                            <v-card
                                v-for="product in filteredProducts"
                                :key="product.id"
                                :product_data="product"
                                @productClick="productClick"
                            />
                        </div>
                        <div v-else class="catalog__carts__produkcts">
                            <v-order
                                v-for="product in filteredProducts"
                                :key="product.id"
                                :product_data="product"
                                @orderClick="orderClick"
                            />
                        </div>
                    </div>
                    <div v-else class="error">
                        По результатом поиска нечего не найдено
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import vCard from './v-card'
    import vOrder from './v-order'
    import { mapActions, mapGetters } from 'vuex';

    export default{
        name: 'v-catalog',
        components: {
            vCard,
            vOrder
        },
        data() {
            return {
                sortedProducts: [],
                currentTypeFilter: 'stock',
                tabStock: true
            }
        },
        computed: {
            ...mapGetters([
                'PRODUCTS',
                'SEARCH_VALUE'
            ]),
            filteredProducts() {
                if (this.currentTypeFilter) {
                    return this.sortedProducts.filter(product => product.type === this.currentTypeFilter);
                } else {
                    return this.PRODUCTS.products
                }
            },
        },
        methods: {
            ...mapActions([
                'GET_PRODUCTS_FROM_API'
            ]),

            toggleTabStockAndFilterTrue(type){
                this.tabStock = true;
                this.filterByType(type)
            },

            toggleTabStockAndFilter(type){
                this.tabStock = false;
                this.filterByType(type)
            },

            productClick(id) {
                this.$router.push( {name: 'product', query: { 'product': id } })
            },

            orderClick(id) {
                this.$router.push( {name: 'order', query: { 'product': id } })
            },

            filterByType(type) {
                this.currentTypeFilter = type;
            },

            sortProductsBySearchValue(value) {
                this.sortedProducts = [...this.PRODUCTS.products];
                if (value) {
                    this.sortedProducts = this.sortedProducts.filter(item => {
                        return item.name.toLowerCase().includes(value.toLowerCase()) || item.brand.toLowerCase().includes(value.toLowerCase());
                    });
                } else {
                    this.sortedProducts = this.PRODUCTS.products;
                }
            }
        },
        watch: {
            SEARCH_VALUE() {
                this.sortProductsBySearchValue(this.SEARCH_VALUE);
            }
        },
        mounted() {
            this.GET_PRODUCTS_FROM_API()
                .then((response) => {
                    if (response.data) {
                        this.sortProductsBySearchValue(this.SEARCH_VALUE)
                    }
                })
        }
    }
</script>

<style>
    .tab{
        padding: 8px;
        border-radius: 4px;
        background: #2E2E2E;
        color: #898989;
        font-size: 14px;
        font-weight: 700;
        width: 100%;
        display: flex;
        justify-content: center;
        transition: all 0.3s;
    }

    .tab-input:checked+label{
        background-color: white;
        color: black;
    }

    .error{
        font-size: 18px;
        text-align: center;
        margin: 64px 0;
    }

    .v-catalog{
        margin-top: 76.8px;
    }

    .catalog__item{
        display: grid;
        grid-template-columns: 1fr;
        column-gap: 15px;
    }

    .catalog__carts__tabs{
        display: grid;
        align-items: center;
        grid-template-columns: 1fr 1fr;
        column-gap: 8px;
    }

    .catalog__carts__sorting{
        display: flex;
        align-items: center;
        cursor: pointer;
    }

    .catalog__carts__sorting-p{
        font-size: var(--fz18);
        margin-right: 12px;
    }

    .catalog__carts__produkcts{
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        gap: 15px;
        margin-top: 24px;
    }

    .filtre-title{
        font-size: var(--fz14);
        margin: 32px 0 12px;
        color: var(--black-color);
    }

    .input-ranges{
        position: relative;
        margin-top: 12px;
        height: 16px;
    }

    .input-ranges-txt{
        display: grid;
        grid-template-columns: 1fr 1fr;
    }

    .input-ranges-txt p{
        font-size: var(--fz14);
        color: var(--black-color);
    }

    .price-input-range{
        position: absolute;
        left: 0;
        bottom: 0;
        width: 100%;
    }

    .price-input-range::-webkit-slider-thumb {
        z-index: 2;
        position: relative;
    }

    .v-catalog{
        margin-bottom: 64px;
    }

    @media (max-width: 981px) {
        .catalog__carts__produkcts{
            grid-template-columns: 1fr 1fr 1fr;
            gap: 11px;
        }
    }

    @media (max-width: 700px) {
        .catalog__carts__produkcts{
            grid-template-columns: 1fr 1fr;
            gap: 8px;
        }
    }

    @media (max-width: 368px) {
        .card__size-p{
            display: none;
        }
    }
</style>