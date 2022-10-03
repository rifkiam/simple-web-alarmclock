<template>
    <div class="clock-container">
        <h3 id="hours">{{("0" + hours).slice(-2) }}:</h3>
        <h3 id="minutes">{{("0" + minutes).slice(-2) }}:</h3>
        <h3 id="seconds">{{("0" + seconds).slice(-2) }}</h3>
        <div id="date"></div>
    </div>
    <div class="alarmClock">
        <div class="alarms">
            <h2>Set alarm for {{("0"+(getJam)).slice(-2)}}:{{("0"+(getMenit)).slice(-2)}}:{{("0"+(getDetik)).slice(-2)}}</h2>
        </div>
        <div class="alarm-container">
            <select v-model="getJam" id="hour" ref="hour">
                <option value="" disabled>Hou value</option>
                <option v-for="i in 24" v-bind:value="i-1">{{ ("0" + (i-1)).slice(-2) }}</option>
            </select>
            <select v-model="getMenit" id="minute" ref="minute">
                <option value="" disabled>Min value</option>
                <option v-for="i in 60" v-bind:value="i-1">{{ ("0" + (i-1)).slice(-2) }}</option>
            </select>
            <select v-model="getDetik" id="second" ref="second">
                <option value="" disabled>Sec value</option>
                <option v-for="i in 60" v-bind:value="i-1">{{ ("0" + (i-1)).slice(-2) }}</option>
            </select>
        </div>
        <div class="alarmSet" id="alarmSet"></div>
        <button @click="setAlarmTime(alarms.length)">Click to set alarm</button>
        <!-- <button @click="var_dump()">Click to debug alarm array</button> -->
        <!-- <button @click="deleteAlarm()" v-else>Stop Alarm</button> -->
        <!-- <div class="uploadField">
            <label for="file-upload" class="custom-file-upload">
                <i class="fa fa-cloud-upload"></i> Choose custom alarm
            </label>
            <input id="file-upload" type="file" @click="changeAlarm()" accept="audio/*"/>
        </div> -->
        <div class="alarmList">
            <div v-for="(items, index) in alarms" class="alarmListItems"><p>{{ ("0" + (items.hour)).slice(-2) }}:{{ ("0" + (items.minute)).slice(-2) }}:{{ ("0" + (items.second)).slice(-2) }}</p>
                <!-- <button class="alarmOptions" @click="deleteAlarm(index)" v-if="items.goOff">Stop</button> -->
                <button class="alarmOptions" @click="snooze(index)">Snooze</button>
                <button class="alarmOptions" @click="deleteAlarm(index)" v-if="!items.goOff">Delete / Stop</button>
                <!-- <button class="alarmOptions" @click="deleteAlarm(index)" v-if="!items.goOff">Snooze</button> -->
            </div>
            <!-- <li>
                <ul v-for="(items, index) in alarms" class="alarmListItems">{{ index + 1 }}. {{ items.hour }}:{{ items.minute }}:{{ items.second }}<button @click="deleteAlarm(index)">Delete</button></ul>
            </li> -->
        </div>
    </div>
</template>

<script>
    import alarmSound from '../assets/alarm.mp3';
    export default {
        name: "Clock",
        mounted() {
            // this.setTime();
            setInterval(() => {this.setTime(), 1000});
        },
        data() {
            return {
                day: '',
                hours: 0,
                minutes: 0,
                seconds: 0,
                getJam: 0,
                getMenit: 0,
                getDetik: 0,
                // alarmOff: true,
                alarmSound: new Audio(alarmSound),
                // newAlarm: {
                //     time: 0,
                // },
                alarms: [],
                flag: false,
                // diff: 0,
            }
        }, // this.alarms[].
        methods: {
            var_dump() {
                console.log(`${this.alarms}`);
            },
            setTime() {
                const weekday = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
                const date = new Date();
                const yyyy = date.getFullYear();
                let mm = date.getMonth() + 1;
                let dd = date.getDate();
                let day = date.getDay();
                let hours = parseInt(date.getHours());
                let minutes = parseInt(date.getMinutes());
                let seconds = parseInt(date.getSeconds());
                this.day = weekday[day];
                const formattedToday = this.day + ' / ' + dd + ' / ' + mm + ' / ' + yyyy;
                document.getElementById('date').innerHTML = formattedToday;
                this.day = day;
                this.hours = hours;
                this.minutes = minutes;
                this.seconds = seconds;
                // ct: Math.round(date.getTime() / 1000);
            },
            alarmGoOff() {
                // if (this.newAlarm !== null) {
                //     this
                // }
                // this.i.alarmTriggered[alarmOff] = false;
                this.flag = true;
                this.alarmSound.play();
                this.alarmSound.loop();
                console.log(alarm);
                document.getElementById('alarmSet').innerHTML = '!!!!!!!!!!!!!';
            },
            deleteAlarm(index) {
                // this.alarms[index].
                this.alarms.splice(index, 1);
                this.alarmSound.pause();
                this.alarmSound.currentTime = 0;
                document.getElementById('alarmSet').innerHTML = '';
                console.log('Alarm deleted');
                this.flag = false;
            },
            setAlarmTime(index) {
                var diff = 0;
                var ts = this.getJam*3600 + this.getMenit*60 + this.getDetik;
                var ct = this.hours*3600 + this.minutes*60 + this.seconds;
                diff = ts - ct;
                var newAlarm = {
                    hour: this.getJam,
                    minute: this.getMenit,
                    second: this.getDetik,
                    goOff: false,
                    time: null,
                    id: 0,
                }
                document.getElementById('alarmSet').innerHTML = `Alarm Time: ${("0" + this.getJam).slice(-2)}:${("0" + this.getMenit).slice(-2)}:${("0" + this.getDetik).slice(-2)}`;
                if (diff < 0) {
                    newAlarm.time = setTimeout(this.alarmGoOff, (86400*1000 + diff*1000));
                    this.alarms.push(newAlarm);
                }
                else {
                    newAlarm.time = setTimeout(this.alarmGoOff, diff*1000);
                    this.alarms.push(newAlarm);
                    // if (this.alarms.isEmpty()) {
                    //     this.alarms.push(newAlarm);
                    // }
                    // else
                    // {
                    //     for (let i = 0; i < this.alarms.length; i++) 
                    //     {
                    //         if (this.alarms[index].time == newAlarm.time) 
                    //         {
                    //             console.log("Insert another value!");
                    //         }
                    //         else
                    //         {
                    //             this.alarms.push(newAlarm);
                    //         }
                    //     }
                    // }
                }
                console.log()
            },
            snooze(index) {
                // console.log(setTimeout(this.var_dump, 10000));
                if (this.flag === false) {
                    document.getElementById('alarmSet').innerHTML = 'Wait for the alarm to go off';
                }
                else{
                    this.flag = false;
                    clearTimeout(this.alarms[index].time);
                    this.alarms[index].time = setTimeout(this.alarmGoOff, 300000);
                    this.alarmSound.pause();
                    this.alarmSound.currentTime = 0;
                    this.alarms[index].minute += 5;
                    document.getElementById('alarmSet').innerHTML = 'Snoozed 5 mins from now';
                    console.log('Alarm snoozed');
                }
            }
            // changeAlarm() {
            //     console.log()
            //     this.newAlarm =  document.getElementById('file-upload').value;
            // }
        }
    }
</script>


<style>
    .active{
        display: contents;
    }

    .alarmListItems {
        padding-top: 5px;
        vertical-align: middle;
        background-color: #242e42;
        height: 120px;
        /* width: 100px; */
        /* width: 120px; */
        width: 49%;
        margin: 10px auto;
        color: white;
        border-radius: 30px;
    }

    input[type="file"] {
    display: none;
    }

    .custom-file-upload {
        border: 2px solid #ccc;
        background-color: #fff;
        border-radius: 12px;
        display: inline-block;
        padding: 3px 9px;
        cursor: pointer;
    }

    #date{
        margin: 10px 0 10px 0;
        color: white;
    }

    #hours, #minutes, #seconds{
        display: inline;
        font-size: 200%;
        color: whitesmoke;
    }

    .clock-container{
        margin: 60px 60px 40px 60px;
        background-color: #2c3e50;
        padding: 20px 60px 20px 60px;
        display: inline-block;
        border-radius: 50px;
    }

    .alarms h2 {
        color: white;
    }

    #alarmSet{
        margin: 8px 0 10px 0;
        color: white;
    }

    .custom-file-upload {
        margin-top: 20px;
    }

    .alarmList {
        margin-bottom: 50px;
    }

    button.alarmOptions {
        margin-left: 23px;
        margin-right: 23px;
    }

    button {
        margin-top: 8px;
        scale: 120%;
        background-color:#2c3e50;
        color: #fff;
        border:none;
        border-radius: 20px;
        padding: 10px;
    }
</style>