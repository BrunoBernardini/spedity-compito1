<template>
    <div id="app">
        <HeaderComp
            @changeView="setView"/>
        <FormComp
            v-if="activeMenu === 1"
            @receiveShippingData="setShippingData"/>
        <RecapComp
            v-if="Object.keys(currentShippingData).length > 0 && activeMenu === 2"
            :shippingDataReceived="currentShippingData"/>
    </div>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue';
import FormComp from './components/FormComp.vue';
import RecapComp from './components/RecapComp.vue';

export default {
    name: 'App',
    components: {
        HeaderComp,
        FormComp,
        RecapComp
    },
    data(){
        return{
            currentShippingData: {},
            activeMenu: 1,
        }
    },
    methods: {
        setShippingData(shippingData){
            this.activeMenu = 2;
            this.currentShippingData = shippingData;
        },
        setView(idView){
            this.activeMenu = idView;
        }
    }
}
</script>

<style lang="scss">
@import "./assets/style/vars";
@import "./assets/style/general";
#app {
    min-height: 100vh;
}
</style>
