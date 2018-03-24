<template>
  <div v-if="isAlive" :class="['enemy_' + index]">
    <div @click="onClickHead" class="enemy_head">
    </div>
    <div @click="onClickBody" class="enemy_body">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Enemy',
  props: {
    index: Number,
    increaseScore: Function,
    decreaseLife: Function,
    decreaseEnemy: Function,
    random: Function,
  },
  data(){
    return {
      life: 3,
      isAlive: true,
    }
  },
  methods: {
    onClickHead: function () {
      this.life = 0
    },
    onClickBody: function () {
      this.life -= 1
    },
  },
  watch: {
    life: function(newVal){
      if(newVal <= 0){
        this.increaseScore()
        this.isAlive = false
        this.decreaseEnemy()
      }
    }
  },
  mounted () {
    const createdStyleTag = document.createElement("style");
    const index = this.index;
    const animationName = `enemy_ani${index}`;
    const randomMs = this.random(7000, 50000)
    
    createdStyleTag.textContent = `
      .enemy_${index} {
        position: absolute;
        top: ${this.random(13, 73)}vh;
        left: ${this.random(11, 85)}vw;

        width: 20px;
        height: 20px;
        background-image: url(http://1.bp.blogspot.com/-EuESyz_GuoE/T09_BIntRaI/AAAAAAAACIg/teIvU4p_6JQ/s1600/Zombie.gif);
        background-size: cover;
        
        z-index: ${50000-randomMs};
        animation: ${animationName} ${randomMs/20}s;
      }

      @keyframes ${animationName} { 
        from { }
        to { width: 20000px; height: 20000px }
      }
    `
    document.body.appendChild(createdStyleTag)

    this[`enemy_timer_${index}`] = setTimeout(()=>{
      if(this.isAlive){
        this.decreaseLife()
        this.isAlive = false
        this.decreaseEnemy()
      }
    }, randomMs)
  },
  beforeDestroy (){
    clearTimeout(this[`enemy_timer_${this.index}`])
  },
}
</script>

<style scoped>
.enemy_head {
  width: 100%;
  height: 40%;
  // background-color: blue;
}

.enemy_body {
  width: 100%;
  height: 60%;
  // background-color: gray;
}
</style>