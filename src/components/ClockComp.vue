<template>
    <div class="clock">
        <div class="clock-cell">
            <h1>{{hours}}</h1>
            <span>Ore</span>
        </div>
        <div class="clock-cell">
            <h1>{{minutes}}</h1>
            <span>Minuti</span>
        </div>
        <div class="clock-cell">
            <h1>{{seconds}}</h1>
            <span>Secondi</span>
        </div>
    </div>
</template>

<script>
export default {
    name: "ClockComp",
    props: {
        expireDate: Date
    },
    data(){
        return{
            hours: 0,
            minutes: 0,
            seconds: 0
        }
    },
    mounted(){
        const startDate = new Date();
        let delta = Math.abs(this.expireDate - startDate)/1000;
        this.hours = Math.floor(delta/3600);
        delta -= this.hours*3600;
        this.minutes = Math.floor(delta / 60) % 60;
        delta -= this.minutes*60;
        this.seconds = Math.floor(delta % 60);
        console.log(this.expireDate, startDate);
        let that = this;
        const timer = setInterval(function(){
            if(!that.decreaseTimer()) clearInterval(timer)
        }, 1000);
    },
    methods: {
        decreaseTimer(){
            if(this.seconds === 0){
                if(this.minutes === 0){
                    if(this.hours === 0)
                        return false;
                    else this.hours--;
                    this.minutes = 59;
                }
                else this.minutes--;
                this.seconds = 59;
            }
            else this.seconds--;
            return true;
        }
    }
}
</script>

<style lang="scss" scoped>
@import "../assets/style/vars";
.clock{
    display: flex;
    column-gap: 5px;
    .clock-cell{
        background-color: $box-shadow-color;
        color: white;
        width: 70px;
        text-align: center;
        border-radius: 10px;
    }
}
</style>