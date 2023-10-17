<template>
    <div class="v-cart-item">
        <div class="container">
            <div class="v-cart-item__item">
                <div class="v-cart-item__item__discription">
                    <h4 class="v-cart-item__item__discription-name">{{cart_item_data.name}}</h4>
                    <p class="v-cart-item__item__discription-price">{{cart_item_data.price}} ₽</p>
                </div>
                <div class="v-cart-item__item__btns">
                    <p class="v-cart-item__item__btns-price">{{cart_item_data.brand}}</p>
                    <div class="v-cart-item__item__btns__rgr2g">
                        <button class="krug" v-on:click="decrementItem">-</button>
                        <p>{{cart_item_data.quantity}} шт.</p>
                        <button class="krug" v-on:click="incrementItem">+</button>
                        
                    </div>
                    <p>
                        {{cart_item_data.size}}
                    </p>
                    <button class="v-cart-item__item__btns-delete" @click="deleteFromCart">Удалить</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default{
        name: 'v-cart-item',
        data() {
            return{
                
            }
        },
        props: {
            cart_item_data: {
                type: Object,
                default() {
                    return {}
                }
            }
        },
        
        methods: {
            decrementItem() {
                this.$emit('decrement')
            },
            incrementItem() {
                this.$emit('increment')
            },
            deleteFromCart() {
                this.$emit('deleteFromCart')
            },
        },
    }
</script>

<style>
    .v-cart-item__item__discription-price{
        white-space: nowrap;
        font-size: 18px;
    }

    .v-cart-item__item__discription-name{
        font-size: 18px;
        font-weight: 700;
    }

    .v-cart-item__item{
        padding: 14px 0;
        border-bottom: 1px solid #252525;
    }




    .v-cart-item__img{
        height: 120px;
    }

    .v-cart-item__img img{
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .v-cart-item__item__discription{
        display: flex;
        justify-content: space-between;
    }

    .v-cart-item__item__discription-type{
        font-weight: 400;
        margin-bottom: 2px;
    }

    .v-cart-item__item__discription-brand{
        font-size: var(--fz12);
        color: var(--dark-grey-color);
        margin-bottom: 2px;
    }

    .v-cart-item__item__discription-size{
        font-size: var(--fz12);
        color: var(--dark-grey-color);
    }

    .v-cart-item__item__discription-data{
        position: absolute;
        bottom: 0;
        font-size: var(--fz12);
        color: var(--dark-grey-color);
        margin-bottom: 12px;
    }

    .v-cart-item__item__btns{
        margin-top: 14px;
        margin-bottom: 24px;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .v-cart-item__item__btns__rgr2g{
        display: flex;
        align-content: center;
        white-space: nowrap;
    }

    .krug{
        background: none;
        border-radius: 20px;
        width: 19px;
        display: flex;
        justify-content: center;
        height: 19px;
        align-items: center;
        margin: 0 8px;
        border: 1px solid white;
        color: white;
    }

    .v-cart-item__item__btns-delete{
        margin-left: 4px;
        background: none;
        border:  0;
        color: var(--red-color);
        text-decoration: underline;
    }

    @media (max-width: 700px) {
        .v-cart-item__item__btns__rgr2g p{
            font-size: var(--fz12);
        }

        .v-cart-item__item__btns-delete{
            font-size: var(--fz12);
        }

        .v-cart-item__item{
            grid-template-columns: 68px 1fr 1fr;
        }
    }
</style>