<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players 
      v-bind:isWinner="isWinner"
      v-bind:activePlayer="activePlayer"
      v-bind:scorePlayer="scorePlayer"
      v-bind:currentScore="currentScore"/>
      <Controls 
      v-bind:isPlaying="isPlaying"
      v-bind:finalScore="finalScore"
      v-on:changeFinalScore="changeFinalScore"
      v-on:handleNewGame="handleNewGame"
      v-on:handleRollDice="handleRollDice"
      v-on:handleHoldScore="handleHoldScore"
      />
      <Dices v-bind:dices="dices"/>
      <popup-rule 
      v-bind:finalScore="finalScore"
      v-bind:isOpenPopup="isOpenPopup"
      v-on:handleConfirm="handleConfirm"/>
    </div>
  </div>
</template>

<script>
import Players from './components/Players';
import Controls from './components/Controls';
import Dices from './components/Dices';
import PopupRule from './components/PopupRule';
export default {
  name: "app",
  data() {
    return {
      activePlayer: 1,
      scorePlayer: [11,12],
      currentScore: 3,
      isOpenPopup: false,
      isPlaying : false,
      dices: [5,5],
      finalScore: 10
    };
  },
  components:{
    Players,
    Controls,
    Dices,
    PopupRule
  },
  computed:{
    isWinner(){
      let{scorePlayer, finalScore} = this;
      if(scorePlayer[0] >= finalScore || scorePlayer[1] >= finalScore){
        this.isPlaying = false;
        return true;
      }
      return false;
    }
  },
  methods:{
    changeFinalScore(e){
      var num = parseInt(e.target.value);
      console.log(num);
      if(isNaN(num)){
        this.finalScore = '';
      }else{
        this.finalScore = num;
      }
    },
    handleHoldScore(){
      if(this.isPlaying){
        // this.scorePlayer[this.activePlayer] += this.currentScore;
        let{scorePlayer, activePlayer, currentScore} = this;
        let scoreOld = scorePlayer[activePlayer];

        this.$set(this.scorePlayer,activePlayer,scoreOld + currentScore);
        if(!this.isWinner){
          this.nextPlayer();
        } 
      }else{
        alert('Press "NEW GAME" please!');
      }
    },
    nextPlayer(){
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleNewGame(){
      this.isOpenPopup = true;
    },
    handleConfirm(){
      this.isOpenPopup = false;
      this.isPlaying = true;
      this.activePlayer = 0;
      this.scorePlayer = [0,0];
      this.currentScore = 0;
      this.dices = [1,1];
    },
    handleRollDice(){
      if(this.isPlaying){
        var d1 = Math.floor(Math.random() * 6) + 1;
        var d2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [d1,d2];
        console.log(d1,d2);
        if(d1 === 1 || d2 === 1){
          
          var name = this.activePlayer;
          setTimeout(function(){
            console.log(name);
            alert('Player' + (name + 1)+ ' lost chance');         
          }, 5)
          this.handleHoldScore();
        }
        else{
          this.currentScore += d1 + d2;
        }
      }else{
        alert('Press "NEW GAME" please!');
      }
    }
  }
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;

}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}

</style>
