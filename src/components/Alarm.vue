<template>
    <div class="clock">
        <div class="alarms">
            <h2>Set alarm for {{("0"+(getJam)).slice(-2)}}:{{("0"+(getMenit)).slice(-2)}}</h2>
        </div>
        <div class="alarm-container">
            <select v-model="getJam" id="hour" ref="hour">
                <option value="" disabled>Please select one</option>
                <option v-for="i in 24" v-bind:value="i-1">{{ ("0" + (i-1)).slice(-2) }}</option>
            </select>
            <select v-model="getMenit" id="minute" ref="minute">
                <option value="" disabled>Please select one</option>
                <option v-for="i in 60" v-bind:value="i-1">{{ ("0" + (i-1)).slice(-2) }}</option>
            </select>
        </div>
        <div class="alarmSet" id="alarmSet"></div>
        <button @click="setAlarmTime()">Click to set alarm</button>
    </div>
</template>

<script>
    import alarm from '../assets/alarm.mp3';
    // import Clock from './Clock.vue'

    export default {
        props: ['hours', 'minutes', 'seconds'],
        name: "Alarm",
        data() {
            return {
                getJam: 0,
                getMenit: 0,
                getDetik: 0,
            }
        },
        methods: {
            alarmGoOff() {
                const music = new Audio(alarm);
                music.play();
                if (this.getJam === this.Clock.hours && this.getMenit === this.Clock.minutes && this.getDetik === this.Clock.seconds) {
                    music.play();
                }
            },
            setAlarmTime() {
                document.getElementById('alarmSet').innerHTML = `Current Timestamp ${hours}:${minutes} / Alarm Time: ${this.getJam}:${this.getMenit}`;
                setTimeout(this.alarmGoOff, (this.getJam*60 + this.getMenit*60 + this.getDetik*1000) - (this.Clock.hours*60 + this.Clock.minutes*60 + this.Clock.seconds*1000))
            }
        }
    }
</script>


<style>
    
</style>

