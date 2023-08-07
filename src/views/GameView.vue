<template>

  <div id="pianoKeyPanel" tabindex='1'>
    <div class="non"></div>
    <div v-for="(row, rowIndex) in pianoKeys" :key="rowIndex" class="pianoKeyRow">
      <div
          v-for="(key, keyIndex) in row"
          :key="keyIndex"
          @click="sing(key)"
          tabindex='1'
          class="pianoKeyContainer"
      >
        <div
            :id="'key' + key"
            class="pianoKey"
            :style="{
              backgroundImage: `url('image/${singName[keyIndex]}.jpg')`,

            }"
        ></div>
        <div :id="'circle' + key" class="pianoKeyCircle"></div>
      </div>
    </div>
  </div>

</template>

<script>

export default {
  data() {
    return {
      singName: ["do", "re", "mi", "fa", "so", "la", "ti"],
      pianoKeys: [
        ['q', 'w', 'e', 'r', 't', 'y', 'u'],
        ['a', 's', 'd', 'f', 'g', 'h', 'j'],
        ['z', 'x', 'c', 'v', 'b', 'n', 'm']
      ],
      audio: {}
    };
  },
  mounted() {
    this.setupAudio();
    const panel = document.getElementById("pianoKeyPanel");
    panel.focus();
    panel.addEventListener("keydown", ev => {
      // alert(ev.code.substring(3,4).toLocaleLowerCase());
      // alert(ev.key)
      if (
          ev.key >= 'a'
          && ev.key <= 'z'
          && ev.key !='i'
          && ev.key !='o'
          && ev.key !='p'
          && ev.key !='k'
          && ev.key !='l'
      ) {
        // let key = ev.code.substring(3, 4).toLocaleLowerCase();
        let key = ev.key;
        console.log(key);
        this.sing(key);

      }

      // this.singMusic(ev.code);
    }, "false")
  },
  methods: {
    setupAudio() {
      for (let i = 0; i < 3; i++) {
        for (let o = 0; o < 7; o++) {
          this.audio[this.pianoKeys[i][o]] = new Audio();
          this.audio[this.pianoKeys[i][o]].src = `audio/${this.pianoKeys[i][o]}.mp3`;
        }
      }
    },

    sing(key) {
      // alert(key)
      this.singMusic(key);
    },
    singMusic(key) {
      if (key !== "-") {
        this.keyAnimate(key);
        let music = new Audio();
        music.src = this.audio[key].src;
        music.play();
      }
    },
    keyAnimate(key) {
      let circle = document.getElementById("circle" + key);
      let pianoKey = document.getElementById("key" + key);

      if (circle && pianoKey) {
        circle.classList.toggle("pianoKeyCircleActive");
        pianoKey.classList.toggle("pianoKeyDownActive");
        setTimeout(() => {
          circle.classList.toggle("pianoKeyCircleActive");
          pianoKey.classList.toggle("pianoKeyDownActive");
        }, 1000);
      }
    },

  }
};
</script>

<style>
#pianoKeyPanel {
  width: 100%;
  height: 80%;
  position: fixed;
  margin: 0 auto;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  background-image: url(image/bg.jpg);
  background-size:100% 100%;
  background-attachment:fixed
}

.non{
  height: 40%;
  width: 100%;
}

.pianoKeyRow {
  display: flex;
  justify-content: center;


}

.pianoKeyContainer {
  position: relative;
  display: inline-block;
  width: 100px;
  height: 100px;
  margin: 10px;
  justify-content: center;
  align-items: center;
}

.pianoKey {
  display: inline-block;
  width: 80px;
  height: 80px;
  border: 0px;
  border-radius: 50%;
  margin: 10px;
  background-size: cover;
}

.pianoKeyCircle {
  z-index: -10;
  position: absolute;
  top: 0px;
  width: 92px;
  height: 92px;
  border: 4px solid #000;
  border-radius: 50%;
  opacity: 0;
}

.pianoKeyCircleActive {
  animation: pianoKeyCircleAnimation 1.5s ease-out 1;
}

@keyframes pianoKeyCircleAnimation {
  0% {
    transform: scale(1);
    opacity: 0;
  }
  50% {
    transform: scale(1.4);
    opacity: 1;
  }
  100% {
    transform: scale(1.4);
    opacity: 0;
  }
}

.pianoKeyDownActive {
  animation: pianoKeyDownAnimation 0.5s ease-out 1;
}

@keyframes pianoKeyDownAnimation {
  0% {
    transform: scale(1);
    filter: none;
  }
  50% {
    transform: scale(0.9);
    filter: brightness(80%);
  }
  100% {
    transform: scale(1);
    filter: none;
  }
}

#FileDrop {
  width: 200px;
  margin: 0 auto;
  position: relative;
}
</style>