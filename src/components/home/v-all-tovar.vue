<template>
    <div class="v-popular-tovar">
        <div class="container">
            <h6 class="tovar-title">
                Каталог
            </h6>
            <div  >
                <swiper
                    :modules="modules"
                    :slidesPerView="slidesPerView"
                    :loop="true"
                    :spaceBetween="15"
                    :pagination="{
                        el: '.swiper-pagination',
                        clickable: true,
                    }"
                    :navigation="{
                        nextEl: '.btn-next',
                        prevEl: '.btn-prev',
                    }"
                >
                    <swiper-slide 
                      v-for="product in PRODUCTS"
                      :key="product.id"
                    >
                      <v-card 
                        :product_data="product"
                        @productClick="productClick"
                      />
                    </swiper-slide>
                </swiper>
            </div>
            <div class="btn-catalog__item">
                <router-link :to="{name: 'catalog'}"  class="btn-catalog__item-a">
                    Открыть каталог
                </router-link>
            </div>
            
            <div class="btns-next-prev">
                <svg class="btn-prev" width="20" height="20">
                    <use xlink:href="../../assets/img/sprite.svg#btn-next-prev"></use>
                </svg>
                <svg class="btn-next" width="20" height="20">
                    <use xlink:href="../../assets/img/sprite.svg#btn-next-prev"></use>
                </svg>
            </div>            
        </div>
    </div>
</template>

<script>
    import {Swiper, SwiperSlide} from 'swiper/vue'
    import {Navigation, Pagination} from 'swiper'
    import 'swiper/css'
    import 'swiper/css/pagination'
    import vCard from '../catalog/v-card'
    import { mapActions, mapGetters } from 'vuex';
    import { ref, onMounted, onUnmounted, watch } from 'vue';


    export default{
        name: 'v-popular-tovar',
        components: {
            Swiper, 
            SwiperSlide, 
            vCard
        },
        setup() {
    let slidesPerView = ref(4); // Начальное значение slidesPerView

    const updateSlidesPerView = (width) => {
      // Здесь вы можете определить свою логику для изменения slidesPerView в зависимости от ширины контейнера
      // Ниже приведен пример с простыми условиями
      if (width < 320) {
        slidesPerView.value = 1;
      } else if (width < 700) {
        slidesPerView.value = 2;
      } else if (width < 981) {
        slidesPerView.value = 3;
      } else {
        slidesPerView.value = 4;
      }
    };

    // Используем watch для отслеживания изменений ширины окна
    watch(
      () => window.innerWidth,
      (newWidth) => {
        updateSlidesPerView(newWidth);
      }
    );

    // Вызываем функцию updateSlidesPerView при монтировании компонента
    onMounted(() => {
      updateSlidesPerView(window.innerWidth);
    });

    // Отменяем отслеживание изменений при размонтировании компонента
    onUnmounted(() => {
      window.removeEventListener('resize', updateSlidesPerView);
    });

    return {
      slidesPerView,
      
    };
  },
        data() {
            return {
                modules: [Pagination, Navigation],
                sortedProducts: [],
            }
        },
        computed: {
            ...mapGetters([
                'PRODUCTS',
            ]),
            // filteredProducts() {
            //     if (this.sortedProducts.length) {
            //         return this.sortedProducts
            //     } else {
            //         return this.PRODUCTS
            //     }
            // },
        },
        methods: {
            ...mapActions([
                'GET_PRODUCTS_FROM_API'
            ]),
            // setRangeSlider() {
            //     if (this.minPrice > this.maxPrice) {
            //         let tmp = this.maxPrice;
            //         this.maxPrice = this.minPrice;
            //         this.minPrice = tmp;
            //     }
            //     this.sortByCategories()
            // },
            // sortByCategories() {
            //     let vm = this;
            //     this.sortedProducts = [...this.PRODUCTS]
            //     this.sortedProducts = this.sortedProducts.filter(function (item) {
            //         return item.price >= vm.minPrice && item.price <= vm.maxPrice
            //     })
            // },
            productClick(id) {
                this.$router.push( {name: 'product', query: { 'product': id } })
            }
        },
        mounted() {
            this.GET_PRODUCTS_FROM_API()
                .then((response) => {
                    if (response.data) {
                        console.log('Data arrived!')
                    }
                })
        }
    }
</script>

<style>
.slider-container {
    width: 100%;
  }
    .v-popular-tovar {
        margin-bottom: 120px;
    }

    .tovar-title {
        color: var(--black-color);
        font-size: var(--fz18);
        font-weight: 400;
        text-align: center;
        margin-bottom: 20px;
    }

    .tovars__item {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        grid-column-gap: 15px;
    }

    .btns-next-prev{
        display: flex;
        justify-content: center;
        margin-top: 10px;
    }

    .btn-prev{
        transform: rotate(180deg);
        transition: all .3s;
    }

    .btn-prev:hover{
        opacity: 0.5;
    }

    .btn-next{
        margin-left: 7px;
        transition: all .3s;
    }

    .btn-next:hover{
        opacity: 0.5;
    }

    .btn-catalog__item{
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .btn-catalog__item-a{
        font-size: var(--fz14);
        color: var(--dark-grey-color);
        margin-top: 20px;
    }

    @media (max-width: 981px) {
        
    }
</style>