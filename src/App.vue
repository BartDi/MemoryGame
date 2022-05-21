<template>
  <h1>Memory Game</h1>
  <div id="gamePanel">
    <div v-for="i in 6">
      <img v-if="canCheck[i-1]" @click="unhide(i)" :src="getImgUrl(i-1)" alt="">
      <div v-else class="card"></div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return{
      uncovers: 0,
      cards: {1:'not', 2:'not', 3:'not', 4:'not', 5:'not', 6:'not'} ,
      images: {1:0, 2:0, 3:0},
      src: ['card.jpg', 'card.jpg', 'card.jpg', 'card.jpg', 'card.jpg', 'card.jpg'],
      imagesUrl: {1:'banana.png', 2:'strawberry.png', 3:'cherry.png'},
      detected: [],
      canCheck: [true, true, true, true, true, true],
    }
  },
  methods: {
    unhide(i){
      if(this.detected.length<2){
        this.toggleCard(i);
      }else if(this.detected.length == 2){
        this.toggleCard(this.detected[0]);
        this.toggleCard(i);
      }
    },
    startGame(){
      var rnd = 0;
      var czyDod = false;
      for(var i = 1; i <= 6; i++){
        do{
            rnd = Math.floor(Math.random() * (3 - 1 + 1)) + 1;
            if (this.images[rnd]<2){
              this.cards[i] = this.imagesUrl[rnd];
              this.images[rnd] += 1;
              czyDod = true;
            }
          }while(czyDod == false)
          czyDod=false;
      }

    },
    getImgUrl(i) {
        return require('./assets/'+this.src[i]);
    },
    toggleCard(i){
      if(this.src[i-1]=='card.jpg'){
        this.src[i-1] = this.cards[i];
        this.detected.push(i);

        if(this.detected.length==2){
          if(this.cards[this.detected[0]] == this.cards[this.detected[1]]){
            setTimeout(() => {
            this.canCheck[this.detected[0]-1] = false;
            this.canCheck[this.detected[1]-1] = false; 
              if(!this.canCheck.includes(true)){
                setTimeout(function () {
                return alert('wygrałes');
                }, 200);
              }
            }, 200);
          }

          setTimeout((i) => {
            this.src[this.detected[0]-1] = 'card.jpg';
            this.src[this.detected[1]-1] = 'card.jpg';
            this.detected.splice(0, 2);
          }, 200);
          
        }

      }else{
        this.src[i-1] = 'card.jpg';
        this.detected.splice(this.detected.indexOf(i), 1);
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
  width:80%;
  height:800px;
  margin:auto;
  border:5px solid crimson;
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
</style>

