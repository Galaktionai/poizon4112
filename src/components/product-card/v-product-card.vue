<template>
    <div class="v-product-card">
        <div class="container">
            <div class="product-card__item">
                <router-link :to="{name: 'catalog'}" class="back">
                    <span>Назад</span>
                </router-link>
                <div class="product-card__images">
                    <div>
                        <img class="product-card__images-img" :src="require('../../assets/img/' + product.img)" alt="">
                    </div>
                </div>
                <div class="product-card__txt">
                    <div class="product-card__txt__flex">
                        <span class="product-card__txt-name">{{product.name}}</span>
                        <span class="product-card__txt-price">{{product.price}}</span>
                    </div>
                    <span class="product-card__txt-brand">Размеры: {{product.sizes}}</span>
                    <span class="product-card__txt-brand">{{product.brand}}</span>
                    <span class="product-card__txt-adress">г. Якутск, ул. Стадухина 83/3г, 2 этаж</span>
                </div>
            </div>
            
        </div>
    </div>
</template>

<script>
    import { mapActions, mapGetters } from 'vuex';
 
    export default{
        name: 'v-product-card',
        props: {
            
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

    .back{
        width: 100%;
        background: white;
        font-size: 14px;
        color: black;
        padding: 8px;
        display: flex;
        justify-content: center;
        margin-bottom: 24px;
        border-radius: 4px;
    }

    .product-card__txt__flex{
        display: flex;
        align-items: flex-start;
        justify-content: space-between;
    }

    .product-card__txt-name{
        font-size: 18px;
        font-weight: 700;
    }

    .product-card__txt-price{
        font-size: 18px;
        white-space: nowrap;
    }

    .product-card__txt{
        display: block;
        margin: 24px 0;
    }

    .product-card__txt-brand{
        display: block;
        color: #898989;
        margin: 8px 0;
    }



    .v-product-card{
        margin-top: 76.8px;
        margin-bottom: 64px;
    }

    .product-card__item{
        display: block;
    }

    .product-card__images-img{
        width: 100%; 
        height: 100%;
        display: block;
        margin: 0;
        border: 1px solid var(--grey-color);
        object-fit: cover;
        border-radius: 4px;
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

    .product-card__txt-description{
        font-size: var(--fz14);
        line-height: 1.8;
    }

    .product-card__txt-ul{
        margin: 12px 0;
        line-height: 1.8;
    }

    .product-card__txt-characteristic{
        font-size: var(--fz14);
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
        transition: all 0.3s;
        opacity: 1;
        cursor: pointer;
    }

    .product-card__txt__bottom__btns button:hover{
        opacity: 0.7;
    }

    .product-card__txt__bottom__size-block{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .product-card-size-input{
        overflow: hidden;
        width: 0;
        height: 0;
    }

    @media (max-width: 981px) {

        .product-card__txt-type{
            font-size: 32px;
        }

        .product-card__txt-description{
            font-size: var(--fz12);
        }

        .product-card__txt-ul h3{
            font-size: var(--fz16);
        }

        .product-card__images__other{
            width: 305px;
            grid-template-columns: 1fr 1fr 1fr 1fr; 
        }

        .product-card__txt__bottom{
            position: relative;
        }

        .product-card__txt__bottom__btns button{
            font-size: var(--fz14);
        }
    }

    @media (max-width: 700px) {

        .product-card__txt-type{
            text-align: center;
        }

        .product-card__images__other{
            width: 100%;
        }

        .product-card__txt__bottom__btns{
            display: block;
        }

        .product-card__txt__bottom__size-block{
            margin-bottom: 12px;
        }

        .product-card__txt__bottom__btns button{
            margin: 4px 0 0px;
        }
    }
</style>