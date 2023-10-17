<template>
    <div class="v-card" @click="productClick">
        <div class="card">
            <img class="card-img" :src="require('../../assets/img/' + product_data.img)" alt="">
            <div class="card__item">
                <p class="card__price-p">{{product_data.price}} ₽</p>
                <p class="card__title-p">{{product_data.name}}</p>
                <p class="card__brand-p">{{product_data.brand}}</p>
            </div>
        </div>
    </div>
    
</template>

<script>

    export default{
        name: 'v-card',
        props: {
            product_data: {
                type: Object,
                default() {
                    return{}
                }
            }
        },
        methods: {
            productClick() {
                this.$emit('productClick', this.product_data.id)
            },
        },
    }
</script>

<style>
    .card{
        cursor: pointer;
        opacity: 1;
        transition: all 0.5s;
    }

    .card-img{
        width: 100%;
        height: 100%;
        object-fit: cover;
        border-radius: 4px;
    }
    
    .card:hover{
        opacity: 0.8;
    }

    .card__price-p{
        font-size: 14px;
    }

    .card__title-p{
        font-size: 14px;
        margin: 4px 0;
    }

    .card__brand-p{
        color: #898989;
        font-size: 12px;
        margin-bottom: 2px;
    }
</style>