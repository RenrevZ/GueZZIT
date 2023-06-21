<template>
    <v-sheet class="d-flex justify-center align-center h-screen">
       <v-card
        class="text-h1"
        width="1000"
        rounded
        color="#FF6F00"
      >
      <template v-slot:title>
        <form @submit.prevent="Guest">
        <div class="d-flex flex-column justify-center align-center">
        
          <h3 class="text-h2 font-weight-bold white">
             GuezzIT
          </h3>

          <span v-if="Playing">
            <span v-if="message =='CORRECT!!'" class="mt-3">
                <h1 class="white font-weight-bold bg-yellow-lighten-1 pa-5 rounded-circle">{{ randomNumber  }}</h1>
            </span>
            <span v-else>
                <h1 class="icon">
                    <v-icon
                    size="x-large"
                    color="yellow-lighten-1"
                    icon="mdi-help-circle-outline"
                    ></v-icon>
                </h1>
            </span>
        </span>
        
        <span v-else>
          <h1 class="icon">
            <v-icon
              size="x-large"
              color="yellow-lighten-1"
              icon="mdi-head-question"
            ></v-icon>
          </h1>
       </span>
  
          <v-col
              cols="12"
              sm="6"
            >
            
          </v-col>
  
          <v-col
              cols="12"
              sm="6"
            >
            <span v-if="counter">
              <div class="d-flex flex-column justify-center align-center white">
                  <h4 class="mb-4">Generating a number in:</h4>
                  <h1 ref="counterDiv">{{ counter }}</h1>
              </div>
            </span>
            
            <transition name="alert-transition">
            <span v-if="message">
                <span>
                    <v-alert
                      color="#FB8C00"
                      theme="dark"
                      icon="mdi-alert-circle"
                      class="mb-4 p-a-1 alert"
                      small
                    >
                    <div class="d-flex flex-column justify-center align-center">
                        <h4>{{ message }}</h4>
                    </div>
                    </v-alert>
                </span>
            </span>
            </transition>
            
            <span v-if="message =='CORRECT!!'" class="d-flex flex-column justify-start align-start mb-5 white">
                <span>
                    <small>Number of Attempts made: {{ attempt }}</small>
                </span>
                    <span>
                        <small>Time Occured: {{ TimeLImit }}</small> 
                </span>         
            </span>
  
            <span v-if="GuestNow">
            <span v-if="message =='CORRECT!!'">
                <v-text-field
                label="Input your guest number"
                hint="Choose a number from 1-100"
                v-model="guestNum"
                variant="outlined"
                class="rounded-pill white"
                max="100"
                min="1"
                type="number"
                disabled
                ></v-text-field>
            </span>
            <span v-else>
              <v-text-field
                label="Input your guest number"
                hint="Choose a number from 1-100"
                v-model="guestNum"
                variant="outlined"
                class="rounded-pill white"
                max="100"
                min="1"
                type="number"
              ></v-text-field>
            </span>
  
              <div class="d-flex justify-space-between align-center white">
                <div>
                   <small>Number of Attemps: {{ attempt }}</small>
                </div>
                <div>
                    <small ref="timerDiv">0:00</small>
                </div>
              </div>
          </span>
          </v-col>
  
         
          <v-col cols="12" sm="6" md="4" class="white">
              <span v-if="!Playing">
                <v-btn type="submit" color="#FB8C00" class="white" @click="Play" block rounded="xl" size="x-large">Play</v-btn>
              </span>
  
              <span v-if="message =='CORRECT!!'">
                <v-btn @click="playAgain" color="#FB8C00" class="white" block rounded="xl" size="x-large">Play Again</v-btn>
              </span>
  
              <span v-if="Playing && message !='CORRECT!!'">
                <v-btn type="submit" color="#FB8C00" class="white" block rounded="xl" size="x-large">Guest</v-btn>
              </span>
          </v-col>
        </div>
      </form>
      </template>
    </v-card>
  </v-sheet>
  </template>
  
  <script>
  import{ ref, watchEffect } from 'vue'
  export default {
    name: 'HelloWorld',
    setup(){
        const guestNum = ref('')
        let counter = ref('')
        let  counterDiv = ref('')
        const attempt = ref(0)
        const GuestNow = ref(false)
        const randomNumber = ref('')
        const Playing = ref(false)
        const message = ref('')
        let PlayTime = ref('')
        const timerDiv = ref(null);
        let TimeLImit = ref('')
        const timer = {
          seconds : 0,
          minutes : 0
        }
       
        //== Timer on first play
        watchEffect(() => {
            PlayTime = setInterval(() => {
                timer.seconds++;
                if (timer.seconds == 60) {
                timer.seconds = 0;
                timer.minutes++;
                }
                
                if (timerDiv.value !== null) {
                timerDiv.value.textContent = timer.minutes + ":" + (timer.seconds < 10 ? "0" + timer.seconds : timer.seconds);
                }
            }, 1000);
        });

       
        //=== ON PLAY FUNCTION
        const Play = () => {
          counter.value = 5
          const interval = setInterval(() => {
            counter.value--;
            counterDiv.value.textContent = counter.value.toString();
            if (counter.value === 0) {

              clearInterval(interval);
              GuestNow.value = true
              Playing.value = true

            }
          }, 1000);

          randomNumber.value = Math.floor(Math.random() * 100) + 1;
        }
        
        //==== POP THE VALUE TO ZERO ON A GIVEN TIME
        const popValue = (element) => {
           const messageValue =  setTimeout(() => {
              element.value = ''
            },1000);
  
            if (element.value === 'CORRECT!!') {
              clearTimeout(messageValue);
            }
        }
        
        //=== ON CLICK GUES THE NUMBER FUNCTION
        const Guest = () => {
          
          if(guestNum.value){
              if(guestNum.value == randomNumber.value){
                  message.value = 'CORRECT!!'
                  popValue(message)
                  clearTimeout(PlayTime)
                  TimeLImit.value = timer.minutes + ":" + (timer.seconds < 10 ? "0" + timer.seconds : timer.seconds)
              }
              else if(guestNum.value > randomNumber.value){
                message.value = 'TOO HIGH !!'
                popValue(message)
                attempt.value++
              }
              else{
                message.value = 'TOO LOW !!'
                popValue(message)
                attempt.value++
              }
          }
  
          guestNum.value = ''
        }

        //==== PLAY AGAIN FUNCTION
        const playAgain = () => {
            //== Reset everything
            attempt.value = 0
            message.value = ''
            timer.seconds = 0
            timer.minutes = 0
            guestNum.value = ''

            //== Clear the interval
            clearInterval(PlayTime);

            //=== Update the timer display
            if (timerDiv.value !== null) {
                timerDiv.value.textContent = "0:00";
            }

            //===  Start the interval again
            PlayTime = setInterval(() => {
                timer.seconds++;
                if (timer.seconds == 60) {
                timer.seconds = 0;
                timer.minutes++;
                }

                if (timerDiv.value !== null) {
                timerDiv.value.textContent = timer.minutes + ":" + (timer.seconds < 10 ? "0" + timer.seconds : timer.seconds);
                }
            }, 1000);

            //=== Regenerate new random number
            randomNumber.value = Math.floor(Math.random() * 100) + 1;
            Guest()
        }
  
        console.log(randomNumber.value)
  
       const dataObj = {
            guestNum,
            Play,
            counter,
            attempt,
            GuestNow,
            Playing,
            Guest,
            message,
            popValue,
            timerDiv,
            timer,
            playAgain,
            randomNumber,
            PlayTime,
            TimeLImit,
            counterDiv
        }
  
        return dataObj
    }
  }
  </script>
<style>
.white{
      color: aliceblue;
}
.icon{
      font-size: 100px;
}

.alert-transition-enter-active {
  animation: alert-scale-in 0.3s ease-out;
}

.alert-transition-leave-active {
  animation: alert-scale-out 0.3s ease-in;
}

@keyframes alert-scale-in {
  0% {
    transform: scale(1.5);
    opacity: 0;
  }
  70% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

@keyframes alert-scale-out {
  0% {
    transform: scale(2);
    opacity: 1;
  }
  30% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
    opacity: 0;
  }
}
  </style>
  