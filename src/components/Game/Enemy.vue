<template>
  <div v-if="isAlive" ref="enemy" :class="`enemy_${index}`">
    <div class="enemy_hp">
      <div v-for="i in life" :style="filledHpStyle"></div>
      <div v-for="i in FULL_LIFE-life" :style="emptyHpStyle"></div>
    </div>
    <div @click="onClickHead" class="enemy_head">
    </div>
    <div @click="onClickBody" class="enemy_body">
    </div>
  </div>
</template>

<script>
  import zombieImg from '../../assets/zombie.gif';

  const FULL_LIFE = 3;
  export default {
    name: 'Enemy',
    props: {
      index: Number,
      increaseScore: Function,
      decreaseLife: Function,
      decreaseEnemy: Function,
      random: Function
    },
    data() {
      return {
        FULL_LIFE,
        life: FULL_LIFE,
        isAlive: true,
        filledHpStyle: {
          backgroundColor: 'rgb(180, 55, 139)',
          width: '33%'
        },
        emptyHpStyle: {
          backgroundColor: 'transparent',
          width: '33%'
        }
      };
    },
    methods: {
      onClickHead() {
        this.life = 0;
      },
      onClickBody() {
        this.life -= 1;
      }
    },
    watch: {
      life(newVal) {
        if (newVal <= 0) {
          this.increaseScore();
          this.isAlive = false;
          this.decreaseEnemy();
        }
      }
    },
    mounted() {
      const styleTag = document.createElement("style");
      const index = this.index;
      const animationName = `enemy_ani${index}`;
      const randomMs = this.random(7000, 50000);

      styleTag.textContent = `
      .enemy_${index} {
        position: absolute;
        top: ${this.random(13, 73)}vh;
        left: ${this.random(11, 85)}vw;

        width: 20px;
        height: 20px;
        background-image: url(${zombieImg});
        background-size: cover;
        
        z-index: ${50000 - randomMs};
        animation: ${animationName} ${randomMs / 20}s;
      }

      @keyframes ${animationName} { 
        from { }
        to { width: 20000px; height: 20000px }
      }
    `;
      this.$refs.enemy.appendChild(styleTag);

      this[`enemy_timer_${index}`] = setTimeout(() => {
        if (this.isAlive) {
          this.decreaseLife();
          this.isAlive = false;
          this.decreaseEnemy();
        }
      }, randomMs);
    },
    beforeDestroy() {
      clearTimeout(this[`enemy_timer_${this.index}`]);
    }
  };
</script>

<style scoped>
  .enemy_hp {
    display: flex;
    width: 50%;
    height: 4px;
    margin-top: -4px;
    margin-left: 18%;
    /* align-items: center; */
  }

  .enemy_filled_hp {
    background-color: rgb(180, 55, 139);
  }

  .enemy_empty_hp {
    background-color: transparent;
  }

  .enemy_head {
    width: 100%;
    height: 40%;
    /* // background-color: blue; */
  }

  .enemy_body {
    width: 100%;
    height: 60%;
    /* // background-color: gray; */
  }
</style>
