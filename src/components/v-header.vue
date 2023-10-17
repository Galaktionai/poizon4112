<template>
    <header class="v-header" :class="headerClasses">
        <div class="header">
            <div class="container">
                <div class="header__item">
                    <router-link :to="{name: 'catalog'}" class="logo">
                        LOGO
                    </router-link>
                    <div class="header__tg__item">
                        <a class="header__tg" :class="{ active: isActive }" href="#">
                            <svg width="24" height="24">
                                <use xlink:href="../assets/img/sprite.svg#telegram"></use>
                            </svg>
                            @poizon4112
                        </a>
                    </div>
                    <div class="header__icons">
                        <router-link :class="{ active: isActive }" :to="{name: 'cart-section'}" class="btn-bag">
                            <p class="cart-length">
                                {{CART.length}}
                            </p>
                            <svg width="36" height="36">
                                <use xlink:href="../assets/img/sprite.svg#bag"></use>
                            </svg>
                        </router-link>
                        <input :class="{ active: isActive }" type="search" class="header-search-input" placeholder="Введите поиск" v-model="searchValue">
                        <svg @click="searchBtn(searchValue), isActive = !isActive" width="36" height="36">
                            <use xlink:href="../assets/img/sprite.svg#search"></use>
                        </svg>
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>

<script>
    import { mapActions, mapGetters } from 'vuex';

    export default{
        name: 'v-header',
        data() {
            return {
                isOpen: true,
                searchValue: '',
                isActive: false,
            }
        },
        computed: {
            ...mapGetters([
                'CART',
                'SEARCH_VALUE'
            ])
        },
        methods: {
            ...mapActions([
                'GET_SEARCH_VALUE_TO_VUEX'
            ]),
            searchBtn(value) {
                this.GET_SEARCH_VALUE_TO_VUEX(value);
                this.$router.push('/')
            }
        },
        
    }
</script>

<style>
.v-header{
    position: fixed;
    width: 100%;
    background-color: black;
    top: 0;
    z-index: 100;
}

.btn-bag{
    position: relative;
    display: block;
    height: 36px;
    display: block;
    opacity: 1;
    transition: all 1s;
}

.btn-bag.active{
    opacity: 0;
}

.header__tg.active{
    opacity: 0;
}

.header__icons.active{
    width: 100%;
}

.cart-length{
    position: absolute;
    background-color: #515151;
    color: black;
    padding: 0px 4px;
    border-radius: 4px;
    transform: translateX(-3px) translateY(23px);
    font-size: 12px;
}

.header{
    padding: 8px 0;
    border-bottom: 1px solid #343434;
}
.header__item{
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
}

.header__icons{
    display: flex;
    align-items: center;
    column-gap: 4px;
}

.header__tg__item{
    position: absolute;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.header__tg{
    position: absolute;
    display: flex;
    align-items: center;
    column-gap: 6px;
    font-size: 14px;
    border-radius: 20px;
    background: rgba(38, 164, 226, 0.20);
    padding: 8px;
    opacity: 1;
    transition: all 1s;
}

.header-search-input{
    display: block;
    height: 36px;
    width: 0;
    opacity: 0;
    transition: all 0.3s;
    border: 0;
    position: absolute;
    left: 0;
    z-index: 100;
    outline: 0;
    background: #2E2E2E;
    color: white;
    font-size: 14px;
    padding: 0 8px 0 16px;
    border-radius: 4px;
}

.header-search-input::placeholder{
    color: #898989;
}

.header-search-input::-webkit-search-cancel-button{
    -webkit-appearance: none;
    appearance: none;
    height: 20px;
    width: 20px;
    background-image: url(../assets/img/exit.svg);
    background-size: 20px 20px;
}

.header-search-input.active{
    width: 88.95%;
    opacity: 1;
}
</style>