<template>
  <h1>Memory Game</h1>
  <select name="" id="" @change="startGame(parseInt($event.target.value))">
    <option value="6">3x2</option>
    <option value="12">3x4</option>
    <option value="16">4x4</option>
  </select>
  <div v-if="isFinished" >
    <Score :title="title" :moves="moves" @again="startGame(numberOfCards)"/>
  </div>
  <div id="gamePanel">
    <div v-for="i in numberOfCards" :key="i">
      <img v-if="canCheck[i-1]" @click="unhide(i)" :src="getImgUrl(i)" alt="">
      <div v-else class="card"></div>
    </div>

  </div>

</template>

<script>
import Score from './components/Score.vue'
export default {
  name: 'App',
  components: { Score },
  data() {
    return{
      cards: [] ,
      visible: [],  
      detected: [],
      srcs: ['banana.png', 'cherry.png', 'strawberry.png', 'lemon.jpg', 'pear.jpg', 'apple.jpg', 'watermelon.jpg', 'pineapple.png'],
      canCheck: [],
      isFinished: false,
      title: "Udało Ci się!!",
      moves: 0,
      numberOfCards: 6,
    }
  },
  methods: {
    unhide(i){
      this.moves += 1;
      if(this.detected.length<2){
        this.toggleCard(i);
      }else if(this.detected.length == 2){
        this.toggleCard(this.detected[0]);
        this.toggleCard(i);
      }
    },
    startGame(n = 6){
      this.numberOfCards = n;
      //Memory game require even number of cards
      if(n%2==1){
        return false;
      }
      //changing attributes to default 
      this.isFinished = false;
      this.moves = 0;
      //pushing starting values to arrays
      this.cards.splice(0);
      this.canCheck.splice(0);
      this.visible.splice(0);
      for(var j = 0; j < n; j++){
        this.cards.push(null);
        this.visible.push('card.jpg'); 
        this.canCheck.push(true);       
      }

      //drawing cards
      for(var i = 1; i <= (n/2); i++){
        for(var j = 1; j <= 2; j++){
          var random = Math.floor(Math.random() * (n - 1 + 1)) + 1;
          while(this.cards[random-1]!=null){
            random = Math.floor(Math.random() * (n - 1 + 1)) + 1;
          }
          this.cards[random-1] = this.srcs[i-1];
        }
      }
    },
    getImgUrl(i) {
        return require('./assets/'+this.visible[i-1]);
    },
    toggleCard(i){
      //If image is card, unhide
      if(this.visible[i-1]=='card.jpg'){
        this.visible[i-1] = this.cards[i-1];
        this.detected.push(i);
        if(this.detected.length==2){
          if(this.cards[this.detected[0]-1] == this.cards[this.detected[1]-1]){
              //changing to false canCheck array on guessed cards position
              //guessed cards disappeared
              setTimeout(() => {
                this.canCheck[this.detected[0]-1] = false;
                this.canCheck[this.detected[1]-1] = false; 
              }, 200);

              //check if game is finished
              //if is, component is adding
              setTimeout(() => {
                if(!this.canCheck.includes(true)){
                  this.isFinished = true;
                }
              }, 200);
          }

          //hide detected cards after 1 sec
          setTimeout((i) => {
            this.visible[this.detected[0]-1] = 'card.jpg';
            this.visible[this.detected[1]-1] = 'card.jpg';
            this.detected.splice(0, 2);
          }, 1000);
          
        }

      }
    }
  },
  created: function(){
    this.startGame();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#gamePanel{
  width:98%;
  height:800px;
  margin:auto;
  margin-left:5%;
}
img{
  width:300px;
  float:left;
  height:300px;
  margin-left:50px;
  margin-top:20px;
}
.card{
  width:300px;
  float:left;
  height:300px;
  margin-left:50px;
  margin-top:20px;
  background-color: white;
}
select{
  margin:10px;
  background-color: #03cfb4;
  width: 100px;
  height: 50px;
  text-align: center;
  border: 1px solid #03b9a1;
  border-radius:10px;
  color:white;
  font-size: large;
}
</style>

