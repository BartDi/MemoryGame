<template>
  <select name="" id="" @change="startGame(parseInt($event.target.value))">
    <option value="6">Easy</option>
    <option value="12">Medium</option>
    <option value="16">Hard</option>
  </select>
  <div v-if="isFinished" >
    <Score :title="title" :moves="moves" @again="startGame(numberOfCards)"/>
  </div>
  <div id="gamePanel">
    <div v-for="i in numberOfCards" :key="i" class="imageDiv" :style="{width:width, height:width}">
      <img v-if="canCheck[i-1]" @click="unhide(i)" style="width: 100%; height:100%"  :src="getImgUrl(i)" alt="">
      <img v-else style="width:100%; height:100%;">
    </div>

  </div>

</template>

<script>
import Score from './Score.vue'
export default {
  name: 'App',
  components: {Score},
  data() {
    return{
      cards: [] ,
      visible: [],  
      detected: [],
      srcs: ['banana.jpg', 'lemon.jpg', 'strawberry.jpg', 'cherry.jpg', 'pear.jpg', 'apple.jpg', 'watermelon.jpg', 'pineapple.png'],
      canCheck: [],
      isFinished: false,
      title: "Udało Ci się!!",
      moves: 0,
      numberOfCards: 6,
      width: "",
    }
  },
  methods: {
    /**
     * 
     * param i - clicked card's number
     * 
     * func handle unhiding and hiding cards 
     */
    unhide(i){
      this.moves += 1;
      if(this.detected.length<2){
        this.toggleCard(i);
      }else if(this.detected.length == 2){
        this.toggleCard(this.detected[0]);
        this.toggleCard(i);
      }
    },
    /**
     * func set width variable which is used in cards appearance 
     */
    setSize(){
      switch(this.numberOfCards){
        case 12:
          this.width = Math.round(95/4).toString() + "%";
          break;
        case 16:
          this.width = Math.round(95/4).toString() + "%";
          break;
        default:
          this.width = Math.round(95/3).toString() + "%";
          break;
      }
    },
    /**
     * param n - number of cards
     * 
     * func to start game with default n=6
     */
    startGame(n = 6){
      this.numberOfCards = n;
      this.setSize();
      console.log(this.width);
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
      this.detected.splice(0);
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
    /**
     * 
     * param i - number of card
     * 
     * function return src to image 
     */
    getImgUrl(i) {
        return require('../assets/'+this.visible[i-1]);
    },
    /**
     * 
     * param i - number of card to hide or unhide
     * 
     * func checks if card is to hide or to unhide
     *  
     */
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

<style lang="scss">

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#gamePanel{
  width:70%;
  height:1000px;
  margin:auto;
  border-left: 4px solid crimson;
  border-right: 4px solid crimson;
}
img{
  display: block;
  float:left;
}
.imageDiv{
  border: 4px solid white;
  float:left;
}
.card{
  float:left;
  display: block;
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

