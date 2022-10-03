<template>
    <div class="clock">
        <div class="clock-container">
            <div id="hours">{{ hours }}</div>
            <div id="minutes">{{ minutes }}</div>
            <div id="seconds">{{ seconds }}</div>
        </div>
        <div class="alarms">
            <h2>Alarm is set for {{getJam}}:{{getMenit}}</h2>
        </div>
        <div class="alarm-container">
            <input type="text" v-model.lazy="getJam" v-bind="getJam" placeholder="0" maxlength="2" id="jam">
            <input type="text" v-model.lazy="getMenit" v-bind="getMenit" placeholder="0" maxlength="2" id="menit">
            <!-- <input type="text" v-model.lazy="getDetik" v-bind="getDetik" placeholder="0" maxlength="2" id="detik"> -->
        </div>
        <button @click="setAlarm()">Click to set alarm</button>
        <!-- <audio id="alarmSound" src="../assets/alarm.mp3">
            <source src="../assets/alarm.mp3" type="audio/mpeg">
        </audio> -->
    </div>
</template>

<script>
    import alarm from '../assets/alarm.mp3';

    export default {

        name: "Clock",
        mounted() {
            setInterval(() => {this.setTime(), 1000});
            // this.setAlarm();
        },
        data() {
            return {
                hours: 0,
                minutes: 0,
                seconds: 0,
                getJam: '',
                getMenit: '',
                getDetik: 0,
            }
        },
        methods: {
            setTime() {
                const date = new Date();
                let hours = date.getHours();
                let minutes = date.getMinutes();
                let seconds = date.getSeconds();
                this.hours = hours;
                this.minutes = minutes;
                this.seconds = seconds;
                console.log(`${this.hours}:${this.minutes}:${this.seconds}  ${this.getJam}:${this.getMenit}:${this.getDetik}`);
            },
            setAlarm() {
                // var music = document.getElementById("alarmSound");
                // music.loop = true;
                if (this.getJam === this.hours && this.getMenit === this.minutes && this.getDetik === this.seconds) {
                    const music = new Audio(alarm);
                    music.play();
                }
            },
        }
    }
</script>


<style>
    
</style>