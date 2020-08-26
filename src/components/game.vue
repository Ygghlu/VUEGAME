<template>
  <div v-if="gameStart==true"  class="container">
    <div class="row">
      <div class="col-md-6">
        <img
          class="img-fluid"
          :style="{width: hpplayer +'%'}"
          :src="player[you].src"
          alt="player pic"
        />
      </div>
    </div>
    <div class="row">
      <div class="col-md-6">{{player[you].name}} HP : {{hpplayer}}</div>
      <div class="col-md-6">Enemy HP : {{hpenemy}}</div>
    </div>
    <div class="row" v-if="gameEnd==false">
      <div class="col-md-2">
        <button
          @click="attack(),randomDamage(3,10),randommonDamage(5,15),playerCal(),enemyCal()"
          class="btn btn-info"
        >Attack</button>
      </div>
      <div class="col-md-3">
        <button
          @click="attack(),randomDamage(10,20),randommonDamage(5,15),playerCal(),enemyCal()"
          class="btn btn-danger"
        >Special Attack</button>
      </div>
    </div>

    <div v-if="startatt==true" class="row">
      <p>
        You did {{damage}} damage!
        <br />
        Enemy attack! You got {{monattack}} damage!
      </p>
    </div>
    <div v-if="hpenemy<=0 & hpplayer<=0" v-on="enemyhpSet()">
      <div v-on="playerhpSet()"></div>
      <p v-on="ending()">Draw!</p>
    </div>
    <div v-else-if="hpplayer<=0" v-on="playerhpSet()">
      <p v-on="ending()">You Lost!</p>
    </div>
    <div v-else-if="hpenemy<=0" v-on="enemyhpSet()">
      <p v-on="ending()">You Win!</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["gameStart","you"],
  data: function () {
    return {
      player: [
        {
          name: "placeholder",
          hp: 1,
          src: require("../assets/img/Abigail1sprite.png"),
        },
        {
          name: "Dioscuri",
          hp: 200,
          src: require("../assets/img/Dioscuri_Sprite13.png"),
        },
        {
          name: "Akuta Hinako",
          hp: 80,
          src: require("../assets/img/Yu_Miaoyi_1.png"),
        },
        {
          name: "Abigail William",
          hp: 120,
          src: require("../assets/img/Abigail1sprite.png"),
        },
      ],
      startatt: false,
      gameEnd: false,
      damage: 0,
      hpplayer: 0,
      hpenemy: 100,
      monattack: 0,

    };
  },
  methods: {
    randomDamage: function (min, max) {
      this.damage = Math.max(Math.floor(Math.random() * max) + 1, min);
      //console.log('damage'+this.damage)
      return this.damage;
    },
    randommonDamage: function (minmon, maxmon) {
      this.monattack = Math.max(Math.floor(Math.random() * maxmon) + 1, minmon);
      //console.log('damage'+this.monattack)
      return this.monattack;
    },
    playerCal: function () {
      this.hpplayer = this.hpplayer - this.monattack;
      // console.log('hp'+this.hpplayer)
    },
    enemyCal: function () {
      this.hpenemy = this.hpenemy - this.damage;
      //console.log('boss'+this.hpenemy)
    },
    playerhpSet: function () {
      this.hpplayer = 0;
    },
    enemyhpSet: function () {
      this.hpenemy = 0;
    },
    ending: function () {
      this.gameEnd = true;
    },
    attack: function () {
      this.startatt = true;
    },
    starthp:function (you){
      this.hpplayer=  this.player[you].hp;
      
      console.log(you)

    }
 
}
};
</script>

<style>
</style>