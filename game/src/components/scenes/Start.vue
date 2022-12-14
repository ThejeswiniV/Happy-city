<template>
  <div id="start" class="full-size both-centered">
    <div class="fluid">
      <div id="title" class="space-font pacchiano">
        Happy City
      </div>
      <div class="actions separated-container">
        <div id="subtitle" class="space-font-mono">
          Collaborative simulator of space disasters
        </div>
        <push-button @click="$router.push('/host')" class="space-font-mono" big :disabled="!connected">Host</push-button>
        <push-button @click="joinLobby()" class="space-font-mono" big :disabled="!connected">Join</push-button>
      </div>
    </div>
    <icon @click.native="toggleMusic()" class="mute-icon" :name="playingMusic ? 'volume-down' : 'volume-off'" scale="3"></icon>
    <div id="footer" class="space-font-mono">
      <span>v{{ version }} ~ </span>
      <span>Created by Giuseppe Guerra for ISIS Di Maggio ~ </span>
      <span>Server status:</span>
      <icon v-if="connecting" name="spinner" class="connecting" pulse></icon>
      <icon v-else-if="connected && !connecting" class="online" name="check-circle"></icon>
      <icon v-else-if="!connected && !connecting" class="offline" name="unlink"></icon>
    </div>
  </div>
</template>

<script>
  import { VERSION } from '@/version.js'

  export default {
    data () {
      return {
        playingMusic: this.isBgmPlaying(),
        version: VERSION
      }
    },
    mounted () {
      if (!this.$store.getters.menuMusicInitialized) {
        this.$store.commit('menuMusicInitialized')
      }
    },
    methods: {
      toggleMusic () {
        if (this.isBgmPlaying()) {
          this.stopBgm()
        } else {
          this.playBgm('static/music/menu.mp3')
        }
        this.playingMusic = this.isBgmPlaying()
      },
      joinLobby () {
        this.$router.push('/join')
      }
    },
    computed: {
      connecting () {
        return this.$store.getters.isConnecting
      },
      connected () {
        return this.$store.getters.isConnected
      }
    }
  }
</script>

<style>
  #start {
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #title {
    animation: title-animation 10s ease-in-out alternate infinite;
    min-height: 100px;
    line-height: 1;
    font-size: 500%;
  }

  @keyframes title-animation {
    from {
      font-size: 500%;
      transform: rotate(3deg);
    }
    50% {
      font-size: 480%;
      transform: rotate(0);
    }
    to {
      font-size: 500%;
      transform: rotate(-3deg);
    }
  }

  #subtitle {
    font-size: 125%;
  }

  .actions {
    width: 50%;
    margin: 0 auto;
  }

  .mute-icon {
    position: absolute;
    right: 0;
    bottom: 0;
    margin: 40px;
  }

  #footer {
    position: absolute;
    bottom: 0;
    text-align: center;
    width: 100%;
    margin-bottom: 20px;
    font-size: 80%;
    font-weight: bold;
  }

  #footer>svg {
    vertical-align: middle;
  }

  #footer>.online {
    color: greenyellow;
  }
  #footer>.connecting {
    color: cornflowerblue;
  }
  #footer>.offline {
    color: yellow;
  }

  @keyframes start-fade-in {
    from {
      opacity: 0;
      padding-top: 30px;
    }
    100% {
      opacity: 1;
      padding-top: 0;
    }
  }

  @media screen and (max-width: 800px) {
    .actions {
      width: 95%;
    }

    #title {
      min-height: 70px;
      font-size: 350%;
    }

    #subtitle {
      font-size: 80%;
    }

    @keyframes title-animation {
      from {
        font-size: 350%;
        transform: rotate(3deg);
      }
      50% {
        font-size: 330%;
        transform: rotate(0);
      }
      to {
        font-size: 350%;
        transform: rotate(-3deg);
      }
    }
  }
</style>
