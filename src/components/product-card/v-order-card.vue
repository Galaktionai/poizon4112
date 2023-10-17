<template>
    <div @click="isActiveModal = !isActiveModal" :class="{ active: isActiveModal }" class="modal-sucsess-bacground">
        <div class="modal-sucsess">
            <div>
                <span>
                    Товар добавлен в корзину
                </span>
                <br>
                <router-link :to="{name: 'cart-section'}" class="modal-sucsess--btn-bag">
                    Перейти в корзину
                </router-link>
            </div>
        </div>
        <!-- <div class="modal-sucsess" v-else>
            <svg @click="isActiveModal = !isActiveModal" class="modal-sucsess-svg" width="24" height="24">
                <use xlink:href="../../assets/img/sprite.svg#exit-modal"></use>
            </svg>
            <div>
                <span>
                    Выберите размер
                </span>
            </div>
        </div> -->
    </div>
    <div class="v-product-card">
        <div class="container">
            <div class="product-card__item">
                <router-link :to="{name: 'catalog'}" class="back">
                    <span>Назад</span>
                </router-link>
                <div class="product-card__images">
                    <img v-if="product.img" class="product-card__images-img" :src=" require('../../assets/img/' + product.img) " alt="">
                </div>
                <div>
                    <div class="order-btns">
                        <button>Купить</button>
                        <button @click="addToCart(), isActiveModal = !isActiveModal">В корзину</button>
                    </div>
                    <div class="btn-group" role="group" id="toolBtns">
                        <input hidden class="product-card-size-input" type="radio" id="sizeS" value="S" name="size" v-model="product.size">
                        <label @click="sizeBtn" :class="active" class="product-card-size-label" for="sizeS">{{product.sizeS}}</label>

                        <input hidden class="product-card-size-input" type="radio" id="sizeM" value="M" name="size" v-model="product.size">
                        <label @click="sizeBtn" :class="active" class="product-card-size-label" for="sizeM">{{product.sizeM}}</label>

                        <input hidden class="product-card-size-input" type="radio" id="sizeL" value="L" name="size" v-model="product.size">
                        <label @click="sizeBtn" :class="active" class="product-card-size-label" for="sizeL">{{product.sizeL}}</label>

                        <input hidden class="product-card-size-input" type="radio" id="sizeXL" value="XL" name="size" v-model="product.size">
                        <label @click="sizeBtn" :class="active" class="product-card-size-label" for="sizeXL">{{product.sizeXL}}</label>
                    </div>
                    <div class="order__flex">
                        <h4>{{product.name}}</h4>
                        <p>{{product.price}}</p>
                    </div>
                    <p class="order-brand">{{product.brand}}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import { mapActions, mapGetters } from 'vuex';
 
    export default{
        name: 'v-product-card',
        components: {
            
        },
        props: {
            cart_data: {
                type: Array,
                default() {
                    return[]
                }
            }
        },
        data () {
            return {
                isActiveModal: false
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

<style>
    .order-btns{
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 8px;
        margin-top: 8px;
    }

    .order-btns button{
        padding: 8px;
        border-radius: 4px;
        background: #FFF;
        color: black;
        font-weight: 700;
        border: 0;
        outline: 0;
    }

    .btn-group{
        margin-top: 8px;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        gap: 8px;
    }

    .order__flex{
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        margin-top: 18px;
        margin-bottom: 8px;
    }

    .order__flex h4{
        font-size: 18px;
        font-weight: 700;
    }

    .order__flex p{
        white-space: nowrap;
        font-size: 18px;
    }

    .order-brand{
        color: #898989;
    }



    .product-card__images__other{
        margin-top: 15px;
        width: 483px;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr; 
        grid-column-gap: 15px;
    }

    .product-card__images__other img{
        width: 100%;
        height: 68px;
        border: 1px solid var(--grey-color);
        object-fit: cover;
    }

    .product-card__txt{
        position: relative;
    }

    .product-card__txt-type{
        font-size: var(--fz48);
    }

    .product-card__txt-brand{
        font-size: var(--fz20);
        margin-bottom: 12px;
    }

    .product-card__txt-description{
        font-size: var(--fz14);
    }

    .product-card__txt-ul{
        margin: 12px 0;
    }

    .product-card__txt-characteristic{
        margin-left: 18px;
    }

    .product-card__txt__bottom{
        position: absolute;
        bottom: 0;
        width: 100%;
    }

    .product-card__txt__bottom__btns{
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-column-gap: 15px;

    }

    .product-card__txt__bottom__btns button{
        background-color: var(--black-color);
        color: var(--white-color);
        width: 100%;
        padding: 16px;
        border: 0;
        border-radius: 4px;
        font-size: var(--fz16);
        margin: 12px 0 83px;
    }

    .product-card__txt__bottom__size-block{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .product-card-size-label{
        border-radius: 4px;
        background: #252525;
        width: 100%;
        text-align: center;
        padding: 4px;
        cursor: pointer;
        font-size: 16px;
    }

    .product-card-size-input:checked + .product-card-size-label{
        background-color: white;
        color: black;
    }

    .modal-sucsess-bacground{
        top: 0;
        position: absolute;
        width: 100%;
        height: 100vh;
        background-color: rgb(0 0 0 / 83%);
        align-items: center;
        justify-content: center;
        z-index: 100;
        display: none;
    }

    .modal-sucsess-svg{
        position: absolute;
        top: 16px;
        right: 16px;
    }

    .modal-sucsess-bacground.active{
        display: flex;
    }

    .modal-sucsess{
        background-color: #252525;
        position: relative;
        height: 324px;
        width: 100%;
        margin: 24px;
        border-radius: 8px;
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 200;
    }

    .bag-modal-sucsess{
        background-color: #252525;
        position: relative;
        height: 275px;
        width: 100%;
        border-radius: 8px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .modal-sucsess--btn-bag{
        margin-top: 24px;
        text-align: center;
        width: 100%;
        display: inline-block;
        padding: 8px;
        border: 1px solid white;
        border-radius: 24px;
        font-size: 12px;
    }
</style>