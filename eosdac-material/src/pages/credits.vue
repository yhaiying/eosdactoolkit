<template>
<q-page class="text-white bg-dark">
  <!-- retro style -->
  <div class="retro_bg full-height full-width absolute"  v-bind:class="{ 'fadeit': !isPlaying }" ></div>
  <div v-if="isPlaying" class="q-pa-md ">
    
    <div class="row justify-between q-mb-lg">
      <div class="relative-position retrotext animate-pop" style="font-size:8vw; opacity: 0.7;text-decoration: underline;">CREDITS</div>
      <span>
        <q-btn v-if="isPlaying" color="dark" icon="volume_up" @click="pauseAudio" />
        <q-btn v-else color="dark" icon="volume_off" @click="playAudio" />
      </span>
    </div>
    
    <div class="relative-position retrotext row items-center q-mt-md" >
      <img class="on-left" src="../statics/gifs/heart.gif" style="height:40px" />
      <span style="font-size:60px;opacity: 0.7;color:black">THANK YOU DAC Workers...</span>
    </div>
    <div class="row q-pa-lg relative-position retrobox retrotext" style="font-size:20px; color:grey">
      [ {{dac_workers.join(', ')}}, ... ]
    </div>

    <div class="relative-position retrotext row items-center q-mt-md" >
      <img class="on-left" src="../statics/gifs/heart.gif" style="height:40px" />
      <span style="font-size:60px;opacity: 0.7;color:black">AND BPs!!</span>
    </div>
    <div class="row q-pa-lg relative-position retrobox ">
      <partnerbps class="retrotext animate-pop"/>
    </div>

    <div class="relative-position retrotext row items-center q-mt-md" >
      <img class="on-left" src="../statics/gifs/heart.gif" style="height:40px" />
      <span style="font-size:60px;opacity: 0.7;color:black">THANK YOU <span class="animate-blink">COMMUNITY</span>...</span>
    </div>

    <div class="relative-position retrotext row items-center " >
      <span style="font-size:60px;opacity: 0.7;color:black">AND ALSO</span>
      <img class="on-right" src="../statics/gifs/heart.gif" style="height:40px" />
    </div>
    <div class="row q-pa-lg relative-position retrobox retrotext" style="font-size:20px; color:grey">
      [ {{others.join(', ')}}, ... ]
    </div>

    <div class="retrotext relative-position full-width q-mt-xl" style="font-size:20px; text-align:right">© eosDAC</div>

  </div>
  <!-- -- -->

  <!-- eosdac corporate style -->
  <div v-if="!isPlaying" class="q-pa-md animate-fade">
    
    <div class="row justify-between q-mb-md">
      <div class="relative-position q-display-1 ">Credits</div>
      <span>
        <q-btn v-if="isPlaying" color="dark" icon="volume_up" @click="pauseAudio" />
        <q-btn v-else color="primary" icon="volume_off" @click="playAudio" />
      </span>
    </div>

    <div class=" q-pa-lg relative-position round-borders bg-dark2 shadow-5 q-mt-md">
      <div class="q-mb-md">eosDAC Workers</div>
      <div class="text-dimwhite">{{dac_workers.join(', ')}}</div>
    </div>

    <div class=" q-pa-lg q-mt-md relative-position round-borders bg-dark2 shadow-5">
      <div class="q-mb-md">Partner BPs</div>
      <partnerbps/>
    </div>
  
    <div class=" q-pa-lg relative-position round-borders bg-dark2 shadow-5 q-mt-md">
      <div class="q-mb-md">Others</div>
      <div class="text-dimwhite">{{others.join(', ')}}</div>
    </div>

  </div>
  <!-- --- -->
  



</q-page>
</template>

<script>
import credits from '../statics/credits.json'
import Partnerbps from 'components/partner_bp'

const audio = new Audio('../statics/sounds/retro.mp3');
audio.addEventListener('ended', function() {
    this.currentTime = 0;
    this.play();
}, false);

import {
  mapGetters
} from 'vuex'
export default {
  name: 'credits',
  components: {
      Partnerbps

  },
  data() {
    return {
      isPlaying: this.getRetroStyle,
      dac_workers: credits.dac_workers,
      others: credits.others

    }
  },
  computed: {
    ...mapGetters({
      // getAccountName: 'account/getAccountName'
        getRetroStyle: 'usersettings/getRetroStyle'
      })

  },
  methods:{
    playAudio(){
      audio.play();
      this.isPlaying = true;
      this.$store.commit('usersettings/SET_RETROSTYLE', true); //remember in store
    },

    pauseAudio(){
      audio.pause();
      this.isPlaying = false;
      this.$store.commit('usersettings/SET_RETROSTYLE', false);//remember in store
    }

  },

  mounted(){

    if(this.getRetroStyle){
      this.playAudio();
    }
  },

  beforeRouteLeave (to, from, next) {
    audio.pause();
    next();
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=VT323');
.bg-flash-color{
  opacity: 0.4;
  background-image: url('../statics/gifs/colors.gif');
  background-size : cover;
  border-top:2px solid white;
 
}
.retro_bg{
  background-image: url('../statics/gifs/retro_star.gif');
  background-size : cover;
  
}
.fadeit{
  opacity: 0.0;
  transition: opacity 0.4s;
}
.retrotext{
  font-family: 'VT323', monospace;
}
.retrobox{
  
  box-sizing: border-box;
  border: 8px solid green;
  outline-color: red;
  outline-style: dashed;
  outline-width: 8px;
  outline-offset: -8px;
  animation-name: flash_border;
  animation-duration: 2s;
  animation-timing-function: step-end;
  animation-iteration-count: infinite;
  background: rgba(255,255,255,0.7)
}

@keyframes flash_border {
  0% {
    border-color: green;
    outline-color: red;
  }
  25% {
    border-color: orange;
    outline-color:green;
  }
  50% {
    border-color: red;
    outline-color: orange;
  }
}

</style>
