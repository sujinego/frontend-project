<template>

  <div id="StopWatch">
		
		
    <div class="m-3" >
    <h1>StopWatch</h1>
	<div class="m-3">
    {{ hours }}:{{ minutes }}:{{ seconds }}.{{ milisec }} 		       <!--mustache-->
	</div>
    </div>

<div>
    <b-button 
    class="m-1"
    variant="primary" id="btn1" v-on:click="btn1">{{ btn1Text }}
    </b-button>

    <b-button
    class="m-1"
		variant="secondary" id="btn2" v-on:click="btn2">{{ btn2Text }}
    </b-button>
</div>
<div>
</div>
</div>
</template>



<script>

export default {
    data () {
        return {
            btn1Text:'시작',
            btn2Text:'기록',
            state:"init",
            ms:0,
            sec:0,
            min:0,
            hor:0,
            timelog:[], 

            timerId:0, //setInterval startTimer function 호출 변수
            startedTime:0,
            calculatedTime:0,
        }
    },

     computed: {


        hours: function() {
            return this.hor >=10 ? this.hor: '0' + this.hor;
        },
        minutes: function() {
            return this.min >=10 ? this.min: '0' + this.min;        
        },
        seconds: function() {
            return this.sec >=10 ? this.sec: '0' + this.sec;
        }, 
        milisec: function() {
            var milisec = parseInt((this.ms%1000));
            return milisec >=10 ? milisec: '0' + milisec;
        }
	

     },
     methods: {


         btn1: function(){

             if(this.state=="ready"){ // 시작
             start()

             }
             else if(this.state=="started"){ //멈춤 기능
             stop()
             }
             else if(this.state=="stopped"){ //재시작 기능
             }
         },
         
         btn2: function(){
             if(this.state=="started"){ //기록
                measure()
             }
             else if(this.state=="stopped"){ //초기화
             }
         },
         start () {
            this.startedTime = new Date().getTime()
            this.startTimer
         },
         updateTime ()  {
             //경과시간을 측정한다.
             this.calculatedTime = new Date().getTime() - this.startedTime
         },
         startTimer () {
             this.timerId = setInterval(this.updateTime, 10)
         }
        
     }
}
</script>



<style scoped>
</style>