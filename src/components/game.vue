<template>
  <div class>
    <div v-if="gameStart==false" class="inner cover">
      <div class="cover-heading">
        <h1 class="mb-5">Singularity Battle</h1>
        <div class="mb-3 row justify-content-center">
          <div class="col-md-4 align-self-center bg-info rounded-pill">
            <h3 class="lead">How to play</h3>
            <p class="text-break">
              กดปุ่มstart เพื่อสุ่มตัวละครผู้เล่นและศัตรู
              <br />กดปุ่ม attack เพิ่มโจมตีปกติ และspecial attack เพื่อโจมตีแบบรุนแรง
              <br />ฝั่งไหนพลังชีวิตหมดก่อนจะเป็นฝ่ายแพ้
              <br />ตัวละครที่ใกล้ตายจะมีขนาดตัวลดลงเรื่อยๆ
            </p>
          </div>
        </div>
      </div>
      <button
        @click="start(),randomplayer(1,4),starthp(),playSound(enemy[ennum].bgm)"
        class="btn btn-danger"
      >Start</button>
      <h2 class="mt-5">Charactor</h2>
      <div class="row justify-content-center">
        <div class="col-md-10">
          <carousel
            :perPage="4"
            :navigationEnabled="true"
            :loop="true"
            :autoplay="true"
            :autoplayTimeout="5000"
          >
            <slide>
              <img class="img-fluid" :src="player[0].portrait" alt srcset /> Player
              <br />Arjuna<br /> {{player[0].des}}
            </slide>
            <slide>
              <img class="img-fluid" :src="player[1].portrait" alt srcset /> Player
              <br />Dioscuri<br /> {{player[1].des}}
            </slide>
            <slide>
              <img class="img-fluid" :src="player[2].portrait" alt srcset /> Player
              <br />Akuta Hinako<br /> {{player[2].des}}
            </slide>
            <slide>
              <img class="img-fluid" :src="player[3].portrait" alt srcset /> Player
              <br />Arthur Pendragon<br /> {{player[3].des}}
            </slide>
            <slide>
              <img class="img-fluid" :src="enemy[0].portrait" alt srcset /> Enemy
              <br />Yog-Sothoth<br /> {{enemy[0].des}}
            </slide>
            <slide>
              <img class="img-fluid" :src="enemy[1].portrait" alt srcset />Enemy
              <br />Cthugha<br /> {{enemy[1].des}}
            </slide>
            <slide>
              <img class="img-fluid" :src="enemy[2].portrait" alt srcset /> Enemy
              <br />Nyalathotep<br /> {{enemy[2].des}}
            </slide>
            <slide>
              <img class="img-fluid" :src="enemy[3].portrait" alt srcset /> Enemy
              <br />Demonic Bodhisattva of Digital Sea,Beast III/R<br /> {{enemy[3].des}}
            </slide>
          </carousel>
        </div>
      </div>
    </div>
    <div v-if="gameStart==true " class="container justify-content-center">
      <div class="row align-items-end">
        <div class="col-5 h-100 d-inline-block">
          <img
            class="img-fluid"
            :style="{width: hpplayer +'%'}"
            :src="player[num].src"
            alt="player pic"
          />
          <b-progress
            :value="hpplayer"
            :max="player[num].hp"
            :precision="2"
            show-value
            variant="danger"
            animated
          ></b-progress>
        </div>
        <div class="col-2 align-self-center">
          <img class="img-fluid" src="../assets/img/img_418591.png" alt="Versus Icon" />
        </div>
        <div class="col-5 h-100 d-inline-block align-self-end">
          <img
            class="img-fluid"
            :style="{width: hpenemy +'%'}"
            :src="enemy[ennum].src"
            alt="enemy pic"
          />
          <b-progress
            :value="hpenemy"
            :max="enemy[ennum].hp"
            :precision="2"
            show-value
            variant="danger"
            animated
          ></b-progress>
        </div>
      </div>
      <div class="row justify-content-between mb-3">
        <div class="col-5">
          {{player[num].name}}
          <br />
          HP : {{hpplayer}}
        </div>
        <div></div>
        <div class="col-5">
          {{enemy[ennum].name}}
          <br />
          HP : {{hpenemy}}
        </div>
      </div>
      <div v-if="startatt==true" class="row justify-content-center">
        <p>
          You did {{damage}} damage!
          <br />
          {{enemy[ennum].name}} attack! You got {{monattack}} damage!
        </p>
      </div>
        <div v-if="startatt==true" class="row justify-content-center">
          <p v-if="burn==true">You got {{burnstack}} burn wound! Losing {{burndam**burnstack}} hp!! </p>
      </div>
      <div class="row justify-content-center" v-if="gameEnd==false">
        <div class="col-md-2">
          <button
            @click="attack(),randomDamage(3,10),playSound(player[num].attsound),randommonDamage(5,15),playerCal(),enemyCal()"
            class="btn btn-info"
          >Attack</button>
        </div>
        <div class="col-md-3">
          <button
            @click="attack(),randomDamage(10,20),playSound(player[num].specsound),randommonDamage(5,15),playerCal(),enemyCal()"
            class="btn btn-danger"
          >Special Attack</button>
        </div>
      </div>

      <div v-if="hpenemy<=0 & hpplayer<=0" v-on="enemyhpSet()">
        <div v-on="playerhpSet()"></div>
        <p class="font-weight-bold" v-on="ending()">Draw!</p>
      </div>
      <div v-else-if="hpplayer<=0" v-on="playerhpSet()">
        <p class="font-weight-bold" v-on="ending()">You Lost!</p>
      </div>
      <div v-else-if="hpenemy<=0" v-on="enemyhpSet()">
        <p class="font-weight-bold" v-on="ending()">You Win!</p>
      </div>
      <div v-if="gameEnd==true">
        <button class="btn" onclick="window.location.href=window.location.href">
          <img class="w-50 image-fluid" src="../assets/img/BBSkip.png" />
          <br />
          <p class="font-white lead">Restart Game?</p>
        </button>
      </div>
    </div>
  </div>
</template>



<script>
export default {
  data: function () {
    return {
      player: [
        {
          name: "Arjuna",
          hp: 500,
          src: require("../assets/img/Arjuna_Alter_Sprite_3.png"),
          multi: 2.68,
          attsound: require("../assets/sound/arjuna-att.ogg"),
          specsound: require("../assets/sound/arjuna-spec.ogg"),
          portrait: require("../assets/img/Arjuna_(Alter)4.png"),
          des: "เทพผู้มีพลังแข็งแกร่ง",
        },
        {
          name: "Dioscuri",
          hp: 200,
          src: require("../assets/img/Dioscuri_Sprite13.png"),
          multi: 2,
          attsound: require("../assets/sound/dio-att.ogg"),
          specsound: require("../assets/sound/dio-spec.ogg"),
          portrait: require("../assets/img/Dioscuri_4.png"),
          des: "เทพฝาแฝด โจมตี2ครั้งซ้อน",
        },
        {
          name: "Akuta Hinako",
          hp: 80,
          src: require("../assets/img/Yu_Miaoyi_1.png"),
          multi: 4.4,
          attsound: require("../assets/sound/yu-att.ogg"),
          specsound: require("../assets/sound/Yu-attack.ogg"),
          portrait: require("../assets/img/YuMiaoyiStage01.png"),
          des: "แวมไพร์สาว บอบบางแต่พลังโจมตีสูง",
        },
        {
          name: "Arthur Pendragon",
          hp: 250,
          src: require("../assets/img/ArthurSprite3.png"),
          multi: 1.4,
          attsound: require("../assets/sound/art-att.ogg"),
          specsound: require("../assets/sound/art-spec.ogg"),
          portrait: require("../assets/img/Arthur4.png"),
          des: "นักดาบที่บาลานซ์",
        },
      ],
      enemy: [
        {
          name: "Yog-Sothoth's Silver Key",
          hp: 480,
          src: require("../assets/img/AbigailWilliamsStage3_NoEffects.png"),
          multi: 1.2,
          bgm: require("../assets/sound/abby-theme.ogg"),
          portrait: require("../assets/img/abby-pr.png"),
          des: "Ygnailh, ygnaiih thflthkh'ngha",
        },
        {
          name: "Cthugha",
          hp: 150,
          src: require("../assets/img/Yang_Guifei_Sprite_3.png"),
          multi: 1.79,
          bgm: require("../assets/sound/yang-bgm.ogg"),
          portrait: require("../assets/img/Yang_Guifei4.png"),
          des: "Ia, ph'nglui mglw'nafh Fomalhaut n'gha-ghaa naf'lthang",
        },
        {
          name: "Nyalathotep",
          hp: 999,
          src: require("../assets/img/BB_Summer_Boss_Sprite3.png"),
          multi: 0.1,
          bgm: require("../assets/sound/bb-bgm.ogg"),
          portrait: require("../assets/img/BBSummer4.png"),
          des:
            "สุดยอดAI จากซูปเปอร์คอมพิวเตอร์บนดวงจันทร์ผู้รักมนุษยชาติมากจนอยากจะทำลายทิ้ง สนุกกับการเล่นไล่จับกับเหยื่อ จึงออมมือให้เสมอ",
        },
        {
          name: "Demonic Bodhisattva of Digital Sea,Beast III/R",
          hp: 6969,
          src: require("../assets/img/Sesshōin_Kiara_Moon_Cancer_NP_Sprite_3.webp"),
          multi: 0.05,
          bgm: require("../assets/sound/Kiara BGM.ogg"),
          portrait: require("../assets/img/Kiarastage04.png"),
          des:
            "โพธิสัตว์มารแห่งห้วงทะเลลึก เธอไม่ทำร้ายคุณ แต่มนุษย์ทุกคนที่เข้าใกล้เธอจะค่อยๆหลอมละลายเหมือนฟองน้ำที่ค่อยๆจางหายไปในท้องทะเล",
        },
      ],
      startatt: false,
      gameEnd: false,
      damage: 0,
      hpplayer: 0,
      hpenemy: 100,
      monattack: 0,
      gameStart: false,
      num: 0,
      ennum: 0,
      burn:false,
      burndam:2,
      burnstack:0
    };
  },
  methods: {
    randomDamage: function (min, max) {
      this.damage =
        Math.max(Math.floor(Math.random() * max) + 1, min) *
        this.player[this.num].multi;
      //console.log('damage'+this.damage)
      return this.damage;
    },
    randommonDamage: function (minmon, maxmon) {
      this.monattack =
        Math.max(Math.floor(Math.random() * maxmon) + 1, minmon) *
        this.enemy[this.ennum].multi;
      //console.log('damage'+this.monattack)
      if(this.burn==true){
        this.burnstack+=1
      }
      return this.monattack;
    },
    playerCal: function () {
      if(this.ennum==1){
      this.hpplayer = this.hpplayer - (this.monattack+(this.burndam**this.burnstack));
      }else{this.hpplayer = this.hpplayer - this.monattack;}
      
      // console.log('hp'+this.hpplayer)
      return this.hpplayer;
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
      if (this.ennum==1){
        this.burn=true
      }
    },
    starthp: function () {
      this.hpplayer = this.player[this.num].hp;
      this.hpenemy = this.enemy[this.ennum].hp;
      return this.hpplayer, this.hpenemy;
    },
    start() {
      this.gameStart = true;
    },
    randomplayer: function (min, max) {
      this.num = Math.max(Math.floor(Math.random() * max) + 1, min) - 1;

      this.ennum = Math.max(Math.floor(Math.random() * max) + 1, min) - 1;
      return this.ennum, this.num;
    },
    playSound(sound) {
      if (sound) {
        var audio = new Audio(sound);
        audio.play();
      }
    },
  },
};
</script>

<style>
</style>