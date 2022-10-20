<template>
    <section class="container">
        <div class="page-box">
            <div class="row">
                <div class="col-xxl-2 col-md-4 col-6 py-2 pe-0 recap-top-el">
                    <div><i class="fa-solid fa-calendar-day recap-top-icon"></i><span>{{formatDate(shippingDataReceived.expireDate, 0)}}</span></div>
                    <div><i class="fa-solid fa-truck recap-top-icon"></i><span>{{isStandard}}</span></div>
                </div>
                <div class="col-xxl-2 col-md-4 col-6 py-2 pe-0 recap-top-el">
                    <i class="fa-solid fa-dolly recap-top-icon"></i><span>{{shippingDataReceived.start}}</span>
                </div>
                <div class="col-xxl-2 col-md-4 col-6 py-2 pe-0 recap-top-el">
                    <i class="fa-solid fa-truck-arrow-right recap-top-icon"></i><span>{{shippingDataReceived.destination}}</span>
                </div>
                <div class="col-xxl-2 col-md-4 col-6 py-2 pe-0 recap-top-el">
                    <div
                        v-if="shippingDataReceived.boxOrPallet">
                        <i class="fa-solid fa-box recap-top-icon"></i><span>Scatola</span>
                    </div>
                    <div
                        v-else>
                        <i class="fa-solid fa-pallet recap-top-icon"></i><span>Pallet</span>
                    </div>
                    <ul class="m-0 px-3">
                        <li><span>{{shippingDataReceived.weight}} Kg</span></li>
                        <li><span>{{shippingDataReceived.lenght}}*{{shippingDataReceived.width}}*{{shippingDataReceived.height}} cm</span></li>
                        <li><span>Qta: {{shippingDataReceived.quantity}}</span></li>
                    </ul>
                </div>
                <div class="col-xxl-2 col-md-4 col-6 py-2 pe-0 recap-top-el">
                    <span><i class="fa-solid fa-comments-dollar recap-top-icon"></i>{{offers}}</span>
                    <div class="mt-2">
                        <span class="badge rounded-pill text-bg-success custom-badge"><i class="fa-regular fa-clock recap-top-icon"></i>{{formatDate(shippingDataReceived.expireDate, 10)}}, {{shippingDataReceived.expireDate.getHours()}}:{{shippingDataReceived.expireDate.getMinutes()}}</span>
                    </div>
                </div>
                <div class="col-xxl-2 col-md-4 col-6 py-2 d-flex justify-content-end align-items-xxl-start align-items-center recap-top-el">
                    <div>
                        <button
                            type="button"
                            class="btn btn-secondary detail-btn"
                            @click="activeSubMenu = !activeSubMenu"><i class="fa-solid fa-circle-chevron-down recap-top-icon"></i>Dettaglio</button>
                    </div>
                </div>
            </div>
            <div
                class="detail-box"
                v-if="activeSubMenu">
                <h6>Dettagli della tua richiesta:</h6>
                <div class="row">
                    <div class="col-4">
                        <p><b>ID spedizione: </b>{{shippingDataReceived.id}}</p>
                        <p><b>Il tuo messaggio per il trasportatore:<br></b>{{shippingDataReceived.message.trim()||"Nessuno"}}</p>
                        <p>
                            <b>Contenuto: </b>{{shippingDataReceived.type}}<br>
                            <b>Valore: </b>{{value}}&euro;
                        </p>
                    </div>
                    <div class="col-4">
                        <b>{{shippingDataReceived.boxOrPallet ? "Scatola" : "Pallet"}}: </b>
                        <ul>
                            <li>{{shippingDataReceived.weight}}Kg</li>
                            <li>{{shippingDataReceived.lenght}}*{{shippingDataReceived.width}}*{{shippingDataReceived.height}} cm</li>
                            <li><b>Qta</b>: {{shippingDataReceived.quantity}}</li>
                        </ul>
                        <b>Dotazione mezzo:</b>
                        <ul v-if="shippingDataReceived.equipment.filter(element => element.selected).length > 0">
                            <li
                                v-for="singleEquipment in shippingDataReceived.equipment.filter(element => element.selected)"
                                :key="singleEquipment.title">{{singleEquipment.title}}</li>
                        </ul>
                        <p v-else>Nessuna</p>
                    </div>
                    <div class="col-4">
                        <b>Servizi aggiuntivi richiesti:</b>
                        <ul v-if="shippingDataReceived.optionalServices.filter(element => element.selected).length > 0">
                            <li
                                v-for="singleOptional in shippingDataReceived.optionalServices.filter(element => element.selected)"
                                :key="singleOptional.title">{{singleOptional.title}}</li>
                        </ul>
                        <p v-else>Nessuno</p>
                    </div>
                </div>
            </div>
            <div class="clock-container my-4 d-flex justify-content-center">
                <ClockComp
                    :expireDate="shippingDataReceived.expireDate"/>
            </div>
            <p class="under-clock text-center">Abbiamo inviato la tua richiesta ai nostri trasportatori.<br>La maggior parte delle offerte viene inviata entro <b>60 minuti nei giorni lavorativi.</b></p>
            <p class="under-clock text-center">In caso di dubbi chiamaci al <span class="badge rounded-pill text-bg-success custom-badge"><i class="fa-solid fa-phone recap-top-icon"></i> +39 06 56548263</span></p>
        </div>
    </section>
</template>

<script>
import ClockComp from './ClockComp.vue';
export default {
    name: "RecapComp",
    props: {
        shippingDataReceived: Object,
        offer: Number,
        value: Number
    },
    data() {
        return {
            activeSubMenu: false
        };
    },
    computed: {
        shippingDataReactive() {
            return this.shippingDataReceived;
        },
        offers() {
            if (!this.offer || this.offer === 0)
                return "Nessuna offerta ricevuta";
            else if (this.offer && this.offer === 1)
                return "1 offerta ricevuta";
            else
                return `${this.offer} offerte ricevute`;
        },
        isStandard() {
            let flag = true;
            this.shippingDataReceived.optionalServices.forEach(element => {
                if (element.selected)
                    flag = false;
            });
            if (flag)
                return "Standard";
            else
                return "Custom";
        }
    },
    methods: {
        padTo2Digits(num) {
            return num.toString().padStart(2, "0");
        },
        formatDate(date, daysToAdd) {
            return [
                this.padTo2Digits(date.getDate() + daysToAdd),
                this.padTo2Digits(date.getMonth() + 1),
                date.getFullYear(),
            ].join("/");
        }
    },
    components: { ClockComp }
}
</script>

<style lang="scss" scoped>
@import "../assets/style/vars";
section{
    min-height: 100px;
    margin-bottom: 50px;
    .recap-top-el{
        overflow: hidden;
        .recap-top-icon{
            display: inline-block;
            margin-right: 10px;
        }
        span{
            font-size: 14px;
            font-weight: bolder;
        }
    }
    .detail-btn{
        font-size: 12px;
        font-weight: bold;
    }
    span.custom-badge{
        font-size: 10px;
        .recap-top-icon{
            margin-right: 5px;
        }
    }
    .detail-box{
        width: 100%;
        min-height: 100px;
        border-radius: 10px;
        margin-top: 20px;
        padding: 5px 10px;
        background-color: lightgray;
        font-size: 12px;
        h6{
            font-weight: bold;
            margin-bottom: 10px;
        }
    }
    .under-clock{
        display: block;
        font-size: 12px;
    }
    .debug{
        border: 1px solid black;
        min-height: 50px;
    }
}
</style>