<template>
    <div class="v-cart">
        <div class="container">
            <div class="v-cart__forms">
                <router-link :to="{name: 'catalog'}" class="back">
                    <span>Назад</span>
                </router-link>
                <form v-if="!submitted" @submit="submitForm" v-on:submit.prevent="formSumbit" method="POST">
                    <div class="v-cart__forms__grid">
                        <div class="div-form">
                            <input required class="div-form-input" v-model="inputSurname" type="text" placeholder="ФИО">
                        </div>
                        <div class="div-form">
                            <input required class="div-form-input" id="input-number" v-model="inputNumber" type="tel" placeholder="+7 (___) ___-__-__">
                        </div>
                        <div class="div-form">
                            <input required class="div-form-input" v-model="inputTelegram" type="text" placeholder="Имя пользователя @telegram">
                        </div>
                        <div class="div-form">
                            <input required class="div-form-input" v-model="inputinstagraI" type="text" placeholder="Имя пользователя @instagram">
                        </div>
                        <div class="div-form">
                            <input required class="div-form-input" v-model="inputCity" type="text" placeholder="Город">
                        </div>
                    </div>
                    <button type="submit" class="order-btn-check">
                        Оформить заказ
                    </button>
                </form>
                <div v-else class="bag-modal-sucsess">
                    <p>Форма успешно отправлена!</p>
                </div>
            </div>
            <h6 v-if="!cart_data.length" class="h6-title">Нет добавленных в корзину товаров</h6>
        </div>
        <v-cart-item
            v-for="(item, index) in cart_data"
            :key="item.id"
            :cart_item_data="item"
            @deleteFromCart="deleteFromCart(index)"
            @increment="increment(index)"
            @decrement="decrement(index)"
        />
    </div>
</template>

<script>
    import axios from 'axios'
    import vCartItem from './v-cart-item'
    import { mapActions, mapGetters } from 'vuex'
    import Inputmask from 'inputmask';
    

    export default{
        name: 'v-cart',
        components: {
            vCartItem
        },
        data() {
            return {
                inputArea: '',
                inputCity: '',
                inputIndex: '',
                inputStreet: '',
                inputinstagraI: '',
                inputNumber: '',
                inputSurname: '',
                inputName: '',
                inputTelegram: '',

                TOKEN: '6227942019:AAFHIHSzWV_vs4zL-1oOUb29t3gdxtKZTGY',
                CHAT_ID: -977477908,
                URL_API: `https://api.telegram.org/bot6227942019:AAFHIHSzWV_vs4zL-1oOUb29t3gdxtKZTGY/sendMessage`,
                submitted: false
            }
        },
        props: {
            cart_data: {
                type: Array,
                default() {
                    return[
                        
                    ]
                }
            },
        },
        computed: {
            ...mapGetters([
                'PRODUCTS.products',
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
        mounted() {
            var im = new Inputmask("7 (999) 999 99 99");
            im.mask(document.getElementById('input-number'));
        },
        methods: {
            submitForm() {
                // Здесь вы можете выполнить логику отправки формы, например, с использованием AJAX или других методов
                // После успешной отправки, установите submitted в true
                // Например:
                // axios.post('/api/submit', this.formData)
                //   .then(response => {
                //     this.submitted = true;
                //   })
                //   .catch(error => {
                //     console.error('Ошибка при отправке формы', error);
                //   });
                
                // Для этого примера, установим submitted в true сразу
                this.submitted = true;
            },
            ...mapActions([
                'DELETE_FROM_CART',
                'INCREMENT_CART_ITEM',
                'DECREMENT_CART_ITEM'
            ]),
            increment(index){
                this.INCREMENT_CART_ITEM(index)
            },
            decrement(index){
                this.DECREMENT_CART_ITEM(index)
            },
            deleteFromCart(index) {
                this.DELETE_FROM_CART(index)
            },

            // formSumbit() {
            //     axios.post(this.URL_API, {
            //         chat_id: this.CHAT_ID,
            //         parse_mode: 'html',
            //         text: 
            //          "Тип: " + this.cart_data[0].type + "\n"
            //          + "Название: " + this.cart_data[0].brand + "\n"
            //          + "Размер: " + this.cart_data[0].size + "\n"
            //          + "Цена: " + this.cart_data[0].price + "\n"
            //          + "Количество: " + this.cart_data[0].quantity + "\n"
            //          + "\n"
            //          + "Край/Облайсть/Регион: " + this.inputArea + "\n"
            //          + "Город: " + this.inputCity + "\n"
            //          + "Почтовый индекс: " + this.inputIndex + "\n"
            //          + "Улица: " + this.inputStreet + "\n"
            //          + "Квартира: " + this.inputFlat + "\n"
            //          + "Номер телефона: " + this.inputNumber + "\n"
            //          + "Фамилия: " + this.inputSurname + "\n"
            //          + "Имя: " + this.inputName + "\n"
            //          + "Отчество: " + this.inputMiddleName + "\n"
            //          ,
            //     })
            // }
            formSumbit() {
                let text = "";
                this.cart_data.forEach((item) => {
                text += "Ссылка: " + item.url + "\n" +
                        "Название: " + item.name + "\n" +
                        "Брэнд: " + item.brand + "\n" +
                        "Размер: " + item.size + "\n" +
                        "Цена: " + item.price + "\n" +
                        "Количество: " + item.quantity + "\n" +
                        "\n";
                });

                text += "ФИО: " + this.inputSurname + "\n"+
                        "Номер телефона: " + this.inputNumber + "\n" +
                        "Инстаграм: " + this.inputinstagraI + "\n" +
                        "Телеграм: " + this.inputTelegram + "\n" +
                        "Город: " + this.inputCity + "\n";

                axios.post(this.URL_API, {
                    chat_id: this.CHAT_ID,
                    parse_mode: 'html',
                    text: text,
                })
            }
        },
    }
    
</script>

<style>
    .v-cart{
        margin-top: 76.8px;
    }
    
    .v-cart__forms{
        margin-top: 18px;
    }

    .order-btn-check{
        width: 100%;
        background-color: black;
        border: 1px solid white;
        color: white;
        font-size: 14px;
        font-weight: 900;
        padding: 8px;
        border-radius: 4px;
        margin: 8px 0 14px;
        text-transform: uppercase;
    }



    .v-cart h6{
        margin: 32px 0;
    }

    .v-cart-total{
        margin-top: 18px;
        text-align: right;
        font-size: var(--fz14);
        font-weight: 700;
        margin-bottom: 32px;
    }

    .grey-line{
        content: '';
        height: 1px;
        width: 100%;
        background: var(--white-grey-color);
        display: block;
    }

    .v-cart__forms__grid{
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 8px;
    }
    
    .div-form-p{
        font-size: var(--fz14);
        margin-bottom: 6px;
    }

    .div-form-p span {
        color: var(--red-color);
    }

    .div-form-input{
        width: 100%;
        display: block;
        padding: 9px 16px; 
        border-radius: 4px;
        background: #252525;
        border: 0;
        outline: none;
        cursor: pointer;
        color: white;
    }

    .div-form-input::placeholder{
        font-size: var(--fz14);
    }

    .send-btn{
        margin-top: 32px;
        border: 1px solid var(--black-color);
        display: flex;
        border-radius: 4px;
        align-items: center;
    }

    .send-btn p{
        color: var(--dark-grey-color);
        margin-left: 14px;
        font-size: var(--fz14);
    }

    .send-btn a{
        color: var(--dark-grey-color);
        margin-left: 5px;
        font-size: var(--fz14);
        text-decoration: underline;
    }

    .send-btn button{
        background: var(--black-color);
        color: white;
        padding: 12px 25px; 
        margin-left: auto;
        font-size: var(--fz14);
        border: 0;
        border-radius: 0 4px 4px 0;
        cursor: pointer;
    }

    @media (max-width: 700px) {
        .v-cart__forms__grid{
            grid-template-columns: 1fr;
        }

        .send-btn{
            display: block;
            border: 0px solid var(--black-color);
        }

        .send-btn p{
            margin: 0;
        }

        .send-btn button{
            margin-top: 12px;
            width: 100%;
            border-radius: 4px
        }
    }
</style>