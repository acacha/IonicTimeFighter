<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>TIME FIGHTER</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Blank</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-grid>
        <ion-row>
          <ion-col>
            <div>Your score: {{ score }}</div>
          </ion-col>
          <ion-col>
            <div class='text-right'>
              <ion-text>
                Time left: {{ timeLeft }}
              </ion-text>            
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    
      <div id="container">
        <ion-button v-on:click="start()" :disabled="gameDisabled" :color="gameStarted ? 'primary' : 'secondary'">
          {{ gameStarted ? 'TAP ME!' : 'START' }}
        </ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, toastController, IonGrid, IonCol, IonRow,IonButton,IonText } from '@ionic/vue';
import { defineComponent } from 'vue';

const GAME_DURATION = 5

let timer: ReturnType<typeof setTimeout>
let timer2: ReturnType<typeof setTimeout>

export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonGrid,
    IonCol,
    IonRow,
    IonButton,
    IonText
  },
  data () {
    return {
      score: 0,
      timeLeft: GAME_DURATION,
      gameStarted: false,
      gameDisabled: false
    }
  },
  watch: {
    timeLeft(newValue) {
      if(newValue === 0) {
        this.finishGame()
      }
    }
  },
  methods: {
    async openToast(toastMessage: string,toastDuration= 2000) {
      const toast = await toastController
        .create({
          message: toastMessage,
          duration: toastDuration
        })
      return toast.present();
    },
    finishGame() {
      this.openToast(`Game over. Your score: ${this.score}`)
      this.score = 0
      this.timeLeft = GAME_DURATION
      clearTimeout(timer);
      this.gameStarted = false
      this.gameDisabled = true
      timer2 = setTimeout(() => {
        this.gameDisabled = false
      }, 2000)
    },
    countDownTimer() {
        if(this.timeLeft > 0) {
            timer = setTimeout(() => {
                this.timeLeft -= 1
                this.countDownTimer()
            }, 1000)
        }
    },
    start() {
      if (this.gameStarted) {
        this.score += 1
      } else {
        this.countDownTimer()
      }
      this.gameStarted = true
    }
  }
});
</script>

<style scoped>

.text-right {
  text-align: right;
}

#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>