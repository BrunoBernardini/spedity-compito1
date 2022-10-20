<template>
    <section class="container">
        <div class="page-box">
            <h5 class="fw-bolder mb-4">Informazioni sulla spedizione</h5>
            <div class="input-container">
                <i class="fa-solid fa-dolly input-icon"></i>
                <input
                    type="text"
                    class="form-control"
                    :class="showErrors ? (shippingData.start.trim().length === 0 ? 'is-invalid': 'is-valid') : ''"
                    placeholder="Luogo di ritiro"
                    v-model="shippingData.start">
                <span class="ms-2 text-danger fw-bold">*</span>
            </div>
            <div class="input-container">
                <i class="fa-solid fa-truck-arrow-right input-icon"></i>
                <input 
                    type="text"
                    class="form-control"
                    :class="showErrors ? (shippingData.destination.trim().length === 0 ? 'is-invalid': 'is-valid') : ''"
                    placeholder="Luogo di consegna"
                    v-model="shippingData.destination">
                <span class="ms-2 text-danger fw-bold">*</span>
            </div>
            <h5 class="fw-bolder mt-4 mb-0">Merce da spedire</h5>
            <div class="radio-container d-md-block d-flex">
                <input
                    type="radio"
                    class="btn-check"
                    name="options"
                    id="option1"
                    autocomplete="off"
                    @click="shippingData.boxOrPallet = true"
                    checked>
                <label class="btn btn-outline-warning fw-bold me-3" for="option1"><i class="fa-solid fa-box btn-icon"></i> Spedisci scatola</label>
                <input
                    type="radio"
                    class="btn-check"
                    name="options"
                    id="option2"
                    autocomplete="off"
                    @click="shippingData.boxOrPallet = false">
                <label class="btn btn-outline-warning fw-bold" for="option2"><i class="fa-solid fa-pallet btn-icon"></i> Spedisci un pallet</label>
            </div>
            <div class="numbers-container d-lg-flex d-block">
                <div class="weight d-flex align-items-center me-4 mb-lg-0 mb-2">
                    <input
                        type="number"
                        class="form-control form-control-sm me-2"
                        :class="showErrors ? (shippingData.weight === null ? 'is-invalid': 'is-valid') : ''"
                        placeholder="Peso"
                        v-model="shippingData.weight">
                        <div class="fw-bold" for="typeNumber">Kg<span class="ms-1 text-danger">*</span></div>
                </div>
                <div class="dimensions d-md-flex d-block align-items-center">
                    <input
                        type="number"
                        class="form-control form-control-sm me-1"
                        :class="showErrors ? (shippingData.lenght === null ? 'is-invalid': 'is-valid') : ''"
                        placeholder="Lunghezza"
                        v-model="shippingData.lenght">
                    <div class="fw-bold me-1" for="typeNumber">x</div>
                    <input
                        type="number"
                        class="form-control form-control-sm me-1"
                        :class="showErrors ? (shippingData.width === null ? 'is-invalid': 'is-valid') : ''"
                        placeholder="Larghezza"
                        v-model="shippingData.width">
                    <div class="fw-bold me-1" for="typeNumber">x</div>
                    <input
                        type="number"
                        class="form-control form-control-sm me-2"
                        :class="showErrors ? (shippingData.height === null ? 'is-invalid': 'is-valid') : ''"
                        placeholder="Altezza"
                        v-model="shippingData.height">
                    <div class="fw-bold me-2" for="typeNumber">cm<span class="ms-1 text-danger">*</span></div>
                </div>
            </div>
            <div class="typology-quantity d-md-inline-flex d-inline-block mt-2">
                <select
                    class="form-select form-select-sm me-1"
                    :class="showErrors ? (shippingData.type.length === 0 ? 'is-invalid': 'is-valid') : ''"
                    v-model="shippingData.type">
                    <option value="" disabled>Seleziona tipologia merce</option>
                    <option value="Merce Generica">Merce Generica</option>
                    <option value="Alimentare secco">Alimentare secco</option>
                    <option value="Alimentare fresco">Alimentare fresco</option>
                    <option value="Farmaceutica">Farmaceutica</option>
                    <option value="ADR">ADR</option>
                </select>
                <span class="me-4 text-danger fw-bold">*</span>
                <div class="fw-bold me-2" for="typeNumber">Qta: </div>
                <input
                    type="number"
                    class="form-control form-control-sm"
                    placeholder="Quantità"
                    v-model="shippingData.quantity"
                    @click="verifyQuantity()">
            </div>
            <div class="row">
                <div class="col-md-6 col-12">
                    <h5 class="fw-bolder my-4">Dotazione mezzo</h5>
                    <div class="form-check">
                        <input
                            class="form-check-input"
                            type="checkbox"
                            @click="shippingData.equipment[0].selected = !shippingData.equipment[0].selected">
                        <label class="form-check-label" for="flexCheckDefault">
                            Sponda idraulica
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            class="form-check-input"
                            type="checkbox"
                            @click="shippingData.equipment[1].selected = !shippingData.equipment[1].selected">
                        <label class="form-check-label" for="flexCheckChecked">
                            Alza/Abbassa
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            class="form-check-input"
                            type="checkbox"
                            @click="shippingData.equipment[2].selected = !shippingData.equipment[2].selected">
                        <label class="form-check-label" for="flexCheckDefault">
                            Carico laterale
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            class="form-check-input"
                            type="checkbox"
                            @click="shippingData.equipment[3].selected = !shippingData.equipment[3].selected">
                        <label class="form-check-label" for="flexCheckChecked">
                            Refrigerato
                        </label>
                    </div>
                </div>
                <div class="col-md-6 col-12">
                    <h5 class="fw-bolder my-4">Servizi opzionali</h5>
                    <div class="form-check">
                        <input
                            class="form-check-input"
                            type="checkbox"
                            @click="shippingData.optionalServices[0].selected = !shippingData.optionalServices[0].selected">
                        <label class="form-check-label" for="flexCheckDefault">
                            Merce non sovrapponibile
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            class="form-check-input"
                            type="checkbox"
                            @click="shippingData.optionalServices[1].selected = !shippingData.optionalServices[1].selected">
                        <label class="form-check-label" for="flexCheckChecked">
                            Servizio Express (data di ritiro garantita)
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            class="form-check-input"
                            type="checkbox"
                            @click="shippingData.optionalServices[2].selected = !shippingData.optionalServices[2].selected">
                        <label class="form-check-label" for="flexCheckDefault">
                            Avviso telefonico
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            class="form-check-input"
                            type="checkbox"
                            @click="shippingData.optionalServices[3].selected = !shippingData.optionalServices[3].selected">
                        <label class="form-check-label" for="flexCheckChecked">
                            Ritiro e Consegna in fascia oraria
                        </label>
                    </div>
                </div>
            </div>
            <h5 class="fw-bolder my-4">Messaggio per il trasportatore</h5>
            <textarea
                class="form-control"
                placeholder="Messaggio"
                v-model="shippingData.message"></textarea>
            <div class="text-center">
                <button
                    class="btn btn-primary fw-bold mt-3"
                    @click="sendPreventive()">
                    <i class="fa-solid fa-file-lines btn-icon"></i>Richiedi preventivo
                </button>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    name: "FormComp",
    data(){
        return{
            showErrors: false,
            shippingData: {
                id: "",
                expireDate: null,
                start: "",
                destination: "",
                boxOrPallet: true,
                weight: null,
                lenght: null,
                width: null,
                height: null,
                type: "",
                quantity: 1,
                equipment: [
                    {
                        title: "Sponda idraulica",
                        selected: false,
                    },
                    {
                        title: "Alza/Abbassa",
                        selected: false,
                    },
                    {
                        title: "Carico laterale",
                        selected: false,
                    },
                    {
                        title: "Refrigerato",
                        selected: false,
                    },
                ],
                optionalServices: [
                    {
                        title: "Merce non sovrapponibile",
                        selected: false,
                    },
                    {
                        title: "Servizio Express (data di ritiro garantita)",
                        selected: false,
                    },
                    {
                        title: "Avviso telefonico",
                        selected: false,
                    },
                    {
                        title: "Ritiro e Consegna in fascia oraria",
                        selected: false,
                    },
                ],
                message: ""
            }
        }
    },
    methods: {
        sendPreventive(){
            if(this.shippingData.start.trim() != "" &&
                this.shippingData.start.trim() != "" &&
                this.shippingData.weight != null &&
                this.shippingData.lenght != null &&
                this.shippingData.width != null &&
                this.shippingData.height != null &&
                this.shippingData.type != null){
                    this.showErrors = false;
                    this.shippingData.id = this.makeid(8);
                    this.shippingData.expireDate = new Date();
                    this.shippingData.expireDate.setHours(this.shippingData.expireDate.getHours()+1);
                    console.log(this.shippingData.expireDate);
                    const shippingDataToSend = Object.assign({},this.shippingData);
                    shippingDataToSend.equipment = JSON.parse(JSON.stringify(this.shippingData.equipment));
                    shippingDataToSend.optionalServices = JSON.parse(JSON.stringify(this.shippingData.optionalServices));
                    this.$emit("receiveShippingData", shippingDataToSend);
            }
            else{
                this.showErrors = true;
            }  
        },
        verifyQuantity(){
            if(this.shippingData.quantity <= 0) this.shippingData.quantity = 1;
        },
        makeid(length) {
            let result = '';
            let characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
            for (let i = 0; i < length; i++ ) {
                result += characters.charAt(Math.floor(Math.random() * characters.length));
            }
            return result;
        }
    }
}
</script>

<style lang="scss" scoped>
@import "../assets/style/vars";
section{
    .input-container{
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        .input-icon{
            background-color: gold;
            padding: 6px 13px;
            border-radius: 7px;
            font-size: 23px;
            margin-right: 20px;
            cursor: pointer;
            transition: .2s all;
        }
        &:active .input-icon{
            background-color: #ffc107;
        }
    }
    .radio-container,
    .numbers-container{
        padding-top: 25px;
    }
    .radio-container{
        row-gap: 10px;
    }
    .typology-quantity select{
        width: 220px;
    }
    .btn .btn-icon{
        margin-right: 10px;
    }
}
</style>