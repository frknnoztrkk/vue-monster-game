<template>
  <audio id="lobiAudio">
    <source src="../src/assets/sounds/lobi.mp3" type="audio/mpeg">
  </audio>
  <audio id="awp">
    <source src="../src/assets/sounds/awp.mp3" type="audio/mpeg">
  </audio>
  <audio id="ayi3">
    <source src="../src/assets/sounds/ayi3.mp3" type="audio/mpeg">
  </audio>
  <audio id="ayiDead">
    <source src="../src/assets/sounds/ayiDead.mp3" type="audio/mpeg">
  </audio>
  <audio id="ayiSaldir">
    <source src="../src/assets/sounds/ayiSaldir.mp3" type="audio/mpeg">
  </audio>
  <audio id="hadihadi">
    <source src="../src/assets/sounds/hadihadi.mp3" type="audio/mpeg">
  </audio>
  <audio id="oyuncuOlme">
    <source src="../src/assets/sounds/oyuncuOlme.mp3" type="audio/mpeg">
  </audio>
  <audio id="saglik">
    <source src="../src/assets/sounds/saglik.mp3" type="audio/mpeg">
  </audio>
  <AnaEkran v-show="login" :save="Loginsave"></AnaEkran>

  <template v-if="!login">
    <div id="baslik">
      <h1>CANAVAR ÖLDÜRME OYUNU</h1>
    </div>
    <div id="score">

      <div id="myScore">
        <h2>OYUNCU:{{ oyuncuSkor }}</h2>
        <div id="myProgress">
          <div id="myBar" :style="`width:${width}%`">{{ width }}%</div>
        </div>
      </div>
      <div id="dusmanScore">
        <h2>DÜŞMAN:{{ canavarSkor }}</h2>
        <div id="itsProgress">
          <div id="itsBar" :style="`width:${width2}%`">{{ width2 }}%</div>
        </div>
      </div>
    </div>
    <div id="buttons">
      <button class="button button1" @click="vur()">SALDIR</button>
      <button class="button button2" @click="ozelYetenek()">ÖZEL YETENEK</button>
      <button class="button button3" @click="saglık()">SAĞLIK</button>
      <button class="button button4" @click="pesEt()">PES ET</button>
      <button class="button button5" @click="reset()">RESET</button>
    </div>
    <div class="message">
      <div class="card-oyuncu">
        <h2>Oyuncu</h2>
        <div class="oyuncu-message">
          <ul v-for="(log) in oyuncuArray">
            <li :class="log.class">
              {{ log.detail }}
            </li>
          </ul>
        </div>
      </div>
      <div class="card-canavar">
        <h2>Düşman</h2>
        <div class="canavar-message">
          <ul v-for="(log) in canavarArray">
            <li :class="log.class">
              {{ log.detail }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </template>
</template>
<script>
import AnaEkran from './components/anaEkran.vue';
export default {
  components: { AnaEkran },
  data() {
    return {
      width: 100,
      width2: 100,
      oyuncuSkor: 0,
      canavarSkor: 0,
      oyuncuArray: [],
      canavarArray: [],
      oyuncuSaglık: [],
      canavarSaglık: [],
      login: true,
      damageOyuncu: null,
      damageCanavar: null,
      bgClass: "",
      bgClass2: "",
      saglikButton: false
    }
  },
  mounted() {
    const a = document.getElementById("lobiAudio")
    a.play()
  },
  watch: {
    login() {
      if (!this.login) {
        const a = document.getElementById("lobiAudio")

        a.pause()
        a.currentTime = 0
      }
    },
    saldirBtn() {
      if (this.width < 1 || this.width2 < 1) {
        this.saldirBtn = true;
      }
      else {
        this.saldirBtn = false;
      }
    },
  },
  methods: {
    Loginsave(status) {
      this.login = status
    },
    vur() {
      var a = Math.floor(Math.random() * 20);
      var b = Math.floor(Math.random() * 10);
      this.damageOyuncu = 0;
      this.damageCanavar = 0;
      if (a > b) {
        this.bgClass = "loseColor";
        this.bgClass2 = "damageColor"
      } else if (b > a) {
        this.bgClass = "damageColor";
        this.bgClass2 = "loseColor"
      } else if (b == a) {
        this.bgClass = "esitColor"
        this.bgClass2 = "esitColor"
      }
      if (this.width > 0) {
        this.width -= a
        this.canavarArray.unshift({
          class: this.bgClass2,
          damage: a,
          detail: `Canavar Saldırısı (${a})`
        })
        if (this.width <= 0) {
          this.width = 0
          const k = document.getElementById("oyuncuOlme")
          k.play()
          this.arttırCanavar()
        }
      }
      if (this.width2 > 0) {
        this.width2 -= b
        this.oyuncuArray.unshift(
          {
            class: this.bgClass,
            damage: b,
            detail: `Oyuncu Saldırısı (${b})`
          })
        if (this.width2 <= 0) {
          this.width2 = 0
          this.arttırOyuncu()
          const i = document.getElementById("ayiDead")
          i.play()
          document.getElementsByClassName(button1).disabled = true;
        }
      }
      if (a < b) {
        const l = document.getElementById("hadihadi")
        l.play()
      }
      else {
        const a = document.getElementById("ayiSaldir")
        a.play()
      }
      this.damageCanavar = b;
      this.damageOyuncu = a;
      this.gameOver()
    },

    arttırOyuncu() {
      this.oyuncuSkor++

    },
    gameOver() {
      if (this.width < 1) {

        this.canavarArray = [];
        this.oyuncuArray = [];
        this.width = 100;
        this.width2 = 100;
        alert("Kaybettin");

      } if (this.width2 < 1) {
        this.canavarArray = [];
        this.oyuncuArray = [];
        this.width = 100;
        this.width2 = 100;
        alert("Kazandın");
      }

    },
    arttırCanavar() {
      this.canavarSkor++
    },
    saglık() {
      if (this.width == 100 || this.width2 == 100) {
        alert("Daha Fazla Can Basamazsın!");
      } else {
        var a = Math.floor(Math.random() * 20) + 1;
        var b = Math.floor(Math.random() * 10) + 1;
        const i = document.getElementById("saglik")
        i.currentTime = 0
        i.play()

        var sayı = Math.round(Math.random())
        // console.log(sayı)
        if (sayı == 0) {
          var c = Math.floor(Math.random() * 15) + 1;
          if (this.width < 100) {
            this.width += c
            this.oyuncuArray.unshift({
              class: 'esitColor',
              detail: `Oyuncu İlk Yardımı(${c})`
            }),
              this.canavarArray.unshift({
                class: 'whiteColor',
                detail: `Canavar İlk Yardımı (0)`
              })

            if (this.width > 100) {
              this.width = 100
            }
          }
        }
        else {
          var d = Math.floor(Math.random() * 15) + 1;
          if (this.width2 < 100) {
            this.width2 += d
            this.canavarArray.unshift({
              class: 'esitColor',
              detail: `Canavar İlk Yardımı(${d})`
            }),
              this.oyuncuArray.unshift({
                class: 'whiteColor',
                detail: `Oyuncu İlk Yardımı (0)`
              })

            if (this.width2 > 100) {
              this.width2 = 100
            }
          }
        }
      }

    },
    ozelYetenek() {
      var yetenek = Math.round(Math.random())
      if (yetenek == 0) {

        var d = Math.floor(Math.random() * 40) + 10;
        this.width2 -= d
        const f = document.getElementById("awp")
        f.currentTime = 0
        f.play()
        this.oyuncuArray.unshift({
          class: 'ozelColor',
          detail: `Oyuncu Özel Yetenek(${d})`
        }),
          this.canavarArray.unshift({
            class: 'whiteColor',
            detail: `Canavar Özel Yetenek (0)`
          })
        if (this.width2 <= 0) {
          this.width2 = 0
          const r = document.getElementById("ayi3")
          r.currentTime = 0
          r.play()
          this.arttırOyuncu()
        }
      }
      else {
        var e = Math.floor(Math.random() * 30) + 10;
        this.width -= e
        this.canavarArray.unshift({
          class: 'ozelColor',
          detail: `Canavar Özel Yetenek(${e})`
        }),
          this.oyuncuArray.unshift({
            class: 'whiteColor',
            detail: `Oyuncu Özel Yetenek (0)`
          })
        const h = document.getElementById("ayiSaldir")
        h.play()
        if (this.width <= 0) {
          this.width = 0
          const y = document.getElementById("oyuncuOlme")
          y.play()
          this.arttırCanavar()
        }
      }
      this.gameOver()
    },
    pesEt() {
      var sonuc = confirm("Pes etmek istediğinize emin misiniz?");
      console.log(sonuc)
      if (sonuc == true) {
        const a = document.getElementById("oyuncuOlme")
        a.play()

        this.oyuncuArray = [];

        this.canavarArray = [];
        this.arttırCanavar();

        this.width = 100
        this.width2 = 100
      }


    },
    reset() {

      var sonuc = confirm("Oyundan çıkmak istediğinize emin misiniz?");
      if (sonuc == true) {
        const a = document.getElementById("lobiAudio")
        a.play()
        this.login = true
        this.oyuncuArray = [];
        this.canavarArray = [];
        this.width = 100
        this.width2 = 100
        this.canavarSkor = 0
        this.oyuncuSkor = 0
      }

    }
  }
}
</script>
<style scoped>
@media screen and (min-width:700px) {
  #buttons {
    display: flex;
  }
}

@media screen and (max-width:700px) {
  h1 {
    font-size: large;
  }

  #buttons button {
    font-size: smaller !important;
    margin: 2px;
  }

  #buttons .button1 {
    width: 48%;
  }

  #buttons .button2 {
    width: 48%;
    white-space: nowrap;
  }

  #buttons .button3 {
    width: 32%;

  }

  #buttons .button4 {
    width: 32%;

  }

  #buttons .button5 {
    width: 32%;

  }

  #buttons {
    font-size: smaller !important;

  }
}

h1 {
  font-family: fantasy;
  color: #ddd;
}

h2 {
  font-family: monospace;
  font-size: x-large;
}

#score {
  display: flex;
  justify-content: space-evenly;
}

#baslik {
  text-align: center;
  border: 2px;
  background-color: green;
  padding: 10px;
}

#myScore {
  width: 100%;
  text-align: center;
  padding-right: 15px;
  padding-left: 15px;
}

#dusmanScore {
  width: 100%;
  text-align: center;
  padding-right: 15px;
  padding-left: 15px;
}

#myProgress {
  width: 100%;
  background-color: #ddd;
}

#myBar {
  height: 50px;
  background-color: #04AA6D;
  text-align: center;
  line-height: 50px;
  color: white;
  filter: drop-shadow(0px 0px 12px grey);
}

#itsProgress {
  width: 100%;
  background-color: #ddd;
}

#itsBar {
  height: 50px;
  background-color: #04AA6D;
  text-align: center;
  line-height: 50px;
  color: white;
  filter: drop-shadow(0px 0px 12px grey);
}

#buttons {
  margin-top: 20px;
  padding: 20px;
  justify-content: space-evenly;
  background-color: aliceblue;
  box-shadow: 0px 0px 12px grey;
}

ul {
  padding-left: 4px;
  padding-right: 4px
}

#buttons button {
  padding: 20px;
  cursor: pointer;
  border-radius: 8px;
  font-size: larger;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px, rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
}

.button1 {
  background-color: red;
}

.button1:disabled {
  background-color: firebrick;
}

.button2 {
  background-color: orange;
}

.button3 {
  background-color: greenyellow;
}

.button4 {
  background-color: darkturquoise;
}

.button5 {
  background-color: fuchsia;
}

.message {
  background-color: aliceblue;
  box-shadow: 0px 0px 12px grey;
  display: flex;
  text-align: center;
}

.card-oyuncu {
  width: 50%;
}

.card-canavar {
  width: 50%;
}

.canavar-message {
  box-shadow: 0px 0px 12px grey;
  padding-bottom: 1px;
  padding-top: 1px;
}

.oyuncu-message {
  box-shadow: 0px 0px 12px grey;
  padding-bottom: 1px;
  padding-top: 1px;
}

ul {
  list-style-type: none;
  text-align: center;
  font-size: larger;
}

.damageColor {
  background-color: #04AA6D;
  color: white;
}

.loseColor {
  background-color: crimson;
  color: white;
}

.esitColor {
  background-color: yellow;
  color: black;
}

.whiteColor {
  background-color: white;
  color: black
}

.ozelColor {
  background-color: orange;
  color: white;
}
</style>