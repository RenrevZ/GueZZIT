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
        <h1 class="icon">
          <v-icon
            size="x-large"
            color="#FFC107"
            icon="mdi-head-question"
          ></v-icon>
        </h1>
      
        <h3 class="text-h2 font-weight-bold white">
           GuezzIT
        </h3>

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
                <h1 id="counter">{{ counter }}</h1>
            </div>
          </span>
          
          <span v-if="message">
            
              <span>
                  <v-alert
                    color="#FB8C00"
                    theme="dark"
                    icon="mdi-alert-circle"
                    class="mb-4 p-a-1 alert"
                    small
                  >
                  {{ message }}
                  </v-alert>
              </span>
          </span>
        

          <span v-if="GuestNow">
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

            <div class="d-flex justify-space-between align-center white">
              <div>
                 <small>Number of Attemps: {{ attempt }}</small>
              </div>
              <div>
                  <small ref="timer">0:00</small>
              </div>
            </div>
        </span>
        </v-col>

       
        <v-col cols="12" sm="6" md="4" class="white">
            <span v-if="!Playing">
              <v-btn type="submit" color="#FB8C00" @click="Play" block rounded="xl" size="x-large">Play</v-btn>
            </span>

           

            <span v-if="Playing">
              <v-btn type="submit" color="#FB8C00" block rounded="xl" size="x-large">Guest</v-btn>
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
      const attempt = ref(0)
      const GuestNow = ref(false)
      const randomNumber = Math.floor(Math.random() * 100) + 1;
      const Playing = ref(false)
      const message = ref('')
      const timer = {
        seconds : 0,
        minutes : 0
      }

      const timerDiv = ref(null);

      watchEffect(() => {
        setInterval(() => {
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

      const Play = () => {
        counter.value = 5
        const interval = setInterval(() => {
          counter.value--;
          document.getElementById('counter').innerHTML = counter.value.toString();
          if (counter.value === 0) {
            clearInterval(interval);
            GuestNow.value = true
            Playing.value = true
          }
        }, 1000);
      }

      const popValue = (element) => {
         const messageValue =  setTimeout(() => {
            element.value = ''
          },1000);

          if (element.value === 'CORRECT!!') {
            clearTimeout(messageValue);
          }
      }

      const Guest = () => {
        
        if(guestNum.value){
            if(guestNum.value == randomNumber){
                message.value = 'CORRECT!!'
                popValue(message)
            }
            else if(guestNum.value > randomNumber){
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

      console.log(randomNumber)

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
          timer
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
  .alert{
    transition: 0.5s;
  }
</style>
