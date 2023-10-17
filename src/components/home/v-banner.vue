<template>
    <section class="v-banner">
        <div class="container">
            <div class="banner-item">
                <div class="banner__box banner__box__one">
                    <swiper
                        :modules="modules"
                        :loop="true"
                        :autoplay="{
                        delay: 2000,
                        disableOnInteraction: false,
                        pauseOnMouseEnter: true,
                        }"
                    >
                        <swiper-slide v-for="text in swiperTextBase" :key="text.description">
                            <img class="banner__box-img" :src="text.img" alt="img">
                            <h2 class="banner__box-txt">{{text.text}}</h2>
                        </swiper-slide>
                    </swiper>
                </div>
                <div class="banner__box banner__box__two">
                    <div>
                        <h2 class="banner__box-txt">
                            МОДА, ВДОХНОВЛЕННАЯ ТОБОЙ!
                        </h2>
                    </div>
                    <img class="banner__box-img" src="https://i.pinimg.com/564x/ff/4f/55/ff4f5530fc537e39ea3a31a21dcac5ea.jpg" alt="">
                </div>
            </div>
        </div>
    </section>
</template>

<script>    
    import {Swiper, SwiperSlide} from 'swiper/vue'
    import {Autoplay, Pagination} from 'swiper'
    import 'swiper/css'
    import 'swiper/css/pagination'
    import {ref} from 'vue'
    export default{
        name: 'v-banner',
        components: {Swiper, SwiperSlide},
        setup() {
            const swiperTextBase = ref([
            {
                text: '',
                img: 'https://i.pinimg.com/564x/ee/62/59/ee62595d4421f4dd2d429cd7a0d1e194.jpg',
            },
            {
                text: '',
                img: 'https://i.pinimg.com/564x/c0/69/97/c06997e831ba31c9ab409a19d8d3a21e.jpg',
            },
            ]);
            return {modules: [Pagination, Autoplay], swiperTextBase}
        }
    }
</script>

<style>
.swiper {
    width: 100%;
    height: 100%;
}

.v-banner{
    margin: 60px 0 120px;
}

.banner__box-txt{
    text-transform: uppercase;
    font-weight: bold;
    color: var(--black-color);
    position: relative;
    font-size: var(--fz36);
    line-height: 1;
    margin-bottom: 8px;
}

.banner-item{
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-column-gap: 15px;
}

.banner__box{
    position: relative;
    transition: all .3s;
    cursor: pointer;
}

.banner__box__one{
    max-width: 483px;
    min-width: 10px;
    height: 524px;
}

.banner__box__two{
    max-width: 483px;
    min-width: 10px;
    height: 408px;
}

.banner__box:hover{
    opacity: .8;
}

.banner__box-img{
    display: block;
    width: 471px;
    height: 100%;
    object-fit: cover;
    position: absolute;
    z-index: 0;
}

@media (max-width: 981px) {
    .v-banner{
        margin: 30px 0 60px;
    }

    .banner__box-img{
        width: 100%;
    }
}

@media (max-width: 700px){
    .banner-item{
        display: block;
    }

    .banner__box__one{
        max-width: 698px;
        min-width: 296px;
        height: 741px;
    }

    .banner__box__two{
        max-width: 100%;
        min-width: 100%;
        margin-bottom: 100px;
    }
}

@media (max-width: 495px){
    .banner__box__two{
        margin-bottom: 134px;
    }
    
}

@media (max-width: 368px) {
    .banner__box__one{
        min-width: 100%;
        height: 384px;
    }

    .banner__box-txt{
        font-size: var(--fz24);
        margin: 0;
    }

    .banner__box__two{
        margin-bottom: 88px;
    }
}

</style>