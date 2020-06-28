<template>
<div class="all" :class="isClicked ? 'otherBg' : ''" @click="toggleIsClicked">
    <div id="app">
      <header>
        <h1>My Music</h1>
      </header>
      <main>
        <section class="player">
          <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
          <div class="controls">
            <button class="prev" @click="prev">Prev</button>
            <button class="play" v-if="!isPlaying" @click="play">Play</button>
            <button class="pause" v-else @click="pause">Pause</button>
            <button class="next" @click="next">Next</button>
          </div>
        </section>
        <section class="playlist">
          <h3>My Playlist</h3>
          <button v-for="song in songs" :key="song.src" @click="play(song)" :class="song.src == current.src ? 'songPlaying' : 'song'"> 
            {{ song.title }} - {{ song.artist }} 
          </button>
        </section>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      isClicked: false,
      songs: [
        {
          title: 'Electronica',
          artist: 'Matti Paalanen',
          src: require('./assets/Electronica.mp3')
        },
        {
          title: 'Alumo',
          artist: 'Zeitgeist',
          src: require('./assets/Zeitgeist.mp3')
        },
        {
          title: 'Goof',
          artist: 'Binearpilot',
          src: require('./assets/Goof.mp3')
        },
        {
          title: 'Liquid Blue',
          artist: 'The Madpix Project',
          src: require('./assets/LiquidBlue.mp3')
        },
        {
          title: 'The Other Side',
          artist: 'LukHahs',
          src: require('./assets/TheOtherSide.mp3')
        },
        {
          title: 'Wish You Were Here',
          artist: 'The Madpix Project',
          src: require('./assets/WishYouWereHere.mp3')
        },
        {
          title: 'Pangea',
          artist: 'Professor Kliq',
          src: require('./assets/Pangea.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song) {
      if(typeof song.src != 'undefined') {
        this.current = song;
        this.player.src = this.current.src
      }
      this.player.play();
      this.player.addEventListener('ended', function(){
        this.index++;
        if(this.index > this.songs.length -1) {
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    prev() {
      this.index--;
      if(this.index < 0) {
        this.index = this.songs.length -1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if(this.index > this.songs.length -1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    toggleIsClicked() {
      this.isClicked = true;
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    background: url('./assets/bg1.jpg') repeat center;
    background-size: cover;
    font-family: 'montserrat', sans-serif;
  }

  #app {
    background-color: rgba(255, 255, 255, 0.6);
    width: 500px;
    height: 600px;
    border-radius: 25px 0px 25px 0px;
    display:flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    text-align: center;
    padding: 25px 50px;
    box-shadow: 15px 45px 100px rgba(255, 255, 255, 0.25);
  }

  #app:hover {
    box-shadow: 30px 45px 100px rgba(255, 255, 255, 0.25);
    border-radius: 0px 75px 0px 75px;
    transition: 1s;
  }

  .all {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  header h1 {
    color: #042c59;
    text-transform: uppercase;
    font-size: 48px;
    font-weight: 900;
  }

  .player h2 {
    padding: 15px 0;
  }

  .controls button {
    border-radius: 50px;
    background: none;
    padding: 10px 25px;
    font-size: 22px;
    margin: 5px;
  }

  .controls .next:hover{
    margin-left: 20px;
    transition: 1s;
  }

  .controls .prev:hover{
    margin-right: 20px;
    transition: 1s;
  }

  .controls button:hover{ 
    background-color: #042c59;
    color: white;
    transition: 1s;
  }

  .controls button:focus{
    outline: none;
  }

  .playlist h3 {
    font-size: 24px;
    padding: 25px;
  }

  .playlist button {
    background: none;
    border: none;
    font-size: 18px;
    display: block;
    width: 100%;
    padding: 10px 15px;
    border-radius: 15px;
  }

  .playlist button:hover {
    color: #fff;
    transition: .25s;
    background: linear-gradient(to right, #042c59, #61abc2);
  }

  .playlist button:focus {
    outline: none;
  }

  .playlist .songPlaying {
    font-weight: 700;
    color: #fff;
    background: linear-gradient(to right, #042c59, #61abc2);
  }

  .otherBg:hover {
    background: url('./assets/bg2.jpg') repeat center;
    background-size: cover;
    transition: 1s;
    border-radius: 25px 75px 25px 75px;
  }

  .controls .play {
    font-size: 30px;
  }

  .controls .pause {
    font-size: 30px;
  }

  /* .playlist {
    overflow-y: scroll;
    height: 250px;
  }
   */
</style>
