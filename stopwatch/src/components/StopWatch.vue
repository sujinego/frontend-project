<template>

  <div id="StopWatch" style="background-color:gray;">		
        <div class="m-3" > <h1>StopWatch</h1>
                <div class="m-3">
                    <span >{{ hh }}</span>:<span >{{ mm }}</span>:<span >{{ ss }}</span><span>:{{ ms }}</span>
                    </div>
                </div>
                <div>
                    <b-button 
                    class="m-1"
                    variant="danger" id="btn1" v-on:click="btn1" >{{ btn1Text }}</b-button>
                    <b-button
                    class="m-1"
                        variant="primary" id="btn3" v-on:click="btn3">{{ btn3Text }}
                </b-button>
            </div>
            <div>
<ul>
  <li v-for="(item, index) in timelog"
    :key="index"
  >
  {{ index+1 }} : {{  getTimeLog(item) }}
  
  </li>
</ul>
            </div>

        <div>
    </div>
</div>
</template>



<script>

export default {
    data () {
        return {
            btn1Text:"start",
            lapbtn:'lap',
            btn3Text:'reset',
            state:"ready",
            timelog:[], 
            timerId:0, //setInterval startTimer function 호출 변수
            time:0,
            startedTime:0,
            displayTime:0
        }
    },

     computed: {
        hh: function() {
            var hh = Math.floor((this.displayTime / (1000 * 60 * 60)) % 24)
            return hh >= 10 ? hh: '0' + hh
        },
        mm: function() {

            var mm = Math.floor((this.displayTime / (1000 * 60)) % 60)
            return mm >= 10 ? mm: '0' + mm        
        },
        ss: function() {
            var ss = Math.floor((this.displayTime/1000) %60)
            return ss >= 10 ? ss: '0' + ss

        }, 
        ms: function() {
            var ms = Math.floor((this.displayTime%1000)/10)
            return ms >= 10 ? ms: '0' + ms
        }
     },
     methods: {
         btn1: function(){

             if(this.state=="ready"){ // start(시작)
                this.start() 
             }
             else if(this.state=="started"){  //measure(기록)
              this.measure()
             }
         },
         btn3:function(){ //reset(초기화)
            this.state=="stopped"
            this.stop()
             
         },
         updateTime ()  {
             //경과시간을 측정한다.
             this.displayTime = Date.now() - this.startedTime
         },
         start () {
            this.startedTime = Date.now()
            this.startTimer()
            this.state = "started"
            this.btn1Text = "stop"
            
         },
         startTimer () {
             this.timerId = setInterval(this.updateTime, 10)
         },
          getTimeLog(measuedTimedMillis) {
            var hor = Math.floor((measuedTimedMillis / (1000 * 60 * 60)) % 24)
            var min = Math.floor((measuedTimedMillis / (1000 * 60)) % 60)
            var sec = Math.floor((measuedTimedMillis / 1000) % 60);
              var milisec = parseInt((measuedTimedMillis % 1000)/10);
              return (hor >=10 ? hor: '0' + hor )
              + ':' 
              +(min >=10 ? min: '0' + min)
              + ':'
              + (sec >=10 ? sec: '0' + sec)
              + '.'
              + (milisec >=10 ? milisec: '0' + milisec)
        },
         measure() { //요청시점에 기록을 측정한다.
            
           const measuredTimeMillis = (Date.now() - this.startedTime)		
            this.timelog.push(measuredTimeMillis)
            
         },
         stop() {
            clearInterval(this.timerId)
            this.reset()
         },
         reset() {
            this.lapbtn= this.btn1Text
            this.displayTime = 0
            this.state = "ready"
            this.btn1Text = "start"
         }
     }
}
</script>



<style scoped>
</style>