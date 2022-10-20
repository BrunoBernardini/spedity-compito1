<template>
    <header>
        <div class="container d-flex justify-content-between align-items-center">
            <img src="../assets/img/Logo 2.0 EXT-posta.jpg">
            <div>
                <button
                    type="button"
                    class="btn btn-primary btn-sm header-btn"
                    @click="changeView(1)">
                    <i class="fa-solid fa-file-circle-plus header-btn-icon"></i><span class="d-md-inline-block d-none ms-2">Nuova richiesta</span>
                </button>
            </div>
        </div>
    </header>
</template>

<script>
export default {
    name: "HeaderComp",
    methods: {
        changeView(idView){
            this.$emit("changeView", idView);
        }
    }
}
</script>

<style lang="scss" scoped>
@import "../assets/style/vars";
header{
    background-color: white;
    box-shadow: 0 0 20px $box-shadow-color;
    margin-bottom: 80px;
    .header-btn{
        font-weight: bold;
    }
}
</style>