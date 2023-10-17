<template>
    <div class="v-home">
        <v-banner />
        <v-all-tovar />
    </div>
</template>

<script>
    import vBanner from './v-banner'
    import vAllTovar from './v-all-tovar'

    export default {
        name: 'v-home',
        components: {
            vBanner,
            vAllTovar
        }
    }
</script>