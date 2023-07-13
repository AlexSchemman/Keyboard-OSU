<template>
  <v-container>
    <div class="wordBox">
      <div class="wordChosen">
        {{ randomKey }}
      </div>
    </div>
    <div class="title-box">
      <div class="title-text">
        Keyboard OSU
      </div>
      <div class="credit-box">
        <v-icon icon="mdi-github"></v-icon> AlexSchemman 
        <v-icon icon="mdi-instagram"></v-icon> ivy.sysko 
      </div>
    </div>
    <div class="text-center">
      <v-menu
      :close-on-content-click="false"
      >
        <template v-slot:activator="{ props }">
          <v-btn
            v-bind="props"
            :ripple="false"
            elevation="0"
            class="settings-menu"
          >
            <v-icon color="success" icon="mdi-cog"></v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item min-width="400" class="mode-option bottom-border">
            <div class="mode-text">
              Zoom
            </div>
            <v-slider
              v-model="sliderVal"
              append-icon="mdi-magnify-plus-outline"
              prepend-icon="mdi-magnify-minus-outline"
              :ticks="tickLabels"
              show-ticks="always"
              class="slider"
              :min="3.5"
              :max="4"
              :step="0.125"
            ></v-slider>
          </v-list-item>
          <v-list-item min-width="400" class="mode-option bottom-border">
            <div class="mode-text">
              Mode
            </div>
            <v-btn
              @click="modeChange();"
              :disabled="!modeConfig"
              :ripple="false"
              v-bind="props"
            >
              Regular
            </v-btn>
            <v-btn
              @click="modeChange();"
              :disabled="modeConfig"
              :ripple="false"
              v-bind="props"
            >
              Stupid
            </v-btn>
          </v-list-item>
          <v-list-item min-width="400" class="mode-option bottom-border">
            <div class="mode-text">
              Color 
            </div>
            <v-btn
              @click="colorChange();"
              :disabled="!colConfig"
              :ripple="false"
              v-bind="props"
            >
              Helpful
            </v-btn>
            <v-btn
              @click="colorChange();"
              :disabled="colConfig"
              :ripple="false"
              v-bind="props"
            >
              UnHelpful
            </v-btn>
          </v-list-item>
          <v-list-item min-width="400" class="mode-option">
            <div class="mode-text">
              Debug Mode
            </div>
            <v-btn
              @click="debugChange();"
              :disabled="debugConfig"
              :ripple="false"
              v-bind="props"
            >
              On
            </v-btn>
            <v-btn
              @click="debugChange();"
              :disabled="!debugConfig"
              :ripple="false"
              v-bind="props"
            >
              Off
            </v-btn>
          </v-list-item>
        </v-list>
      </v-menu>
    </div>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
  },
  methods: {
    chooseRandom() {
      let rngChoose = this.arrayOfKeys[(this.arrayOfKeys.length-1) * Math.random() | 0]
      this.randomKey = rngChoose==this.randomKey?this.chooseRandom():rngChoose;
      this.randomKey==null?this.chooseRandom():this.$emit('generated-rng-key', this.randomKey);
    },
    modeChange() {
      this.modeConfig = !this.modeConfig;
      this.emitSettings();
    },
    colorChange() {
      this.colConfig = !this.colConfig;
      this.emitSettings();
    },
    debugChange() {
      this.debugConfig = !this.debugConfig;
      this.emitSettings();
    },
    emitSettings() {
      this.$emit('settings-config', {
        'SliderSize': this.sliderVal + 'em',
        'ModeConfig' : this.modeConfig,
        'ColConfig' : this.colConfig,
        'DebugConfig' : this.debugConfig
      })
    }
  },
  watch: {
    sliderVal() {
      this.emitSettings();
    }
  },
  data() {
    return {
      sliderVal: 4,
      modeConfig: false,
      colConfig: false,
      debugConfig: false,
      tickLabels: {
        3.5: '1',
        3.625: '2',
        3.75: '3',
        3.875: '4',
        4: '5',
      },
      arrayOfKeys: [
        '1', '2', '3', '4', '5', '6', '7', '8', '9', '0',
        'Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P',
        'A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L',
        'Z', 'X', 'C', 'V', 'B', 'N', 'M'
      ],
      randomKey: ''
    }
  },
  created() {
    this.chooseRandom();
  },
}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped lang="scss">

.v-container {
  position: relative;
  background-color: rgb(202,186,156);
  border-radius: 1em;
  width: 85%;
  margin-top: 5%;
}

.v-btn {
  margin: 0em 1em;
  width: 12em;
}

.text-center{
  align-self: flex-end;
  display: flex;
  margin-right: -1em;
  right: 0;
  bottom: 0;
  position: absolute;

  .settings-menu {
    width: 3em;
    height: 3em;
    min-width: 3em;
    background-color: #102820;
    border-radius: 1em 0em;

    &:hover {
      .mdi-cog {
        -webkit-transform: rotateZ(360deg);
        -moz-transform: rotateZ(360deg);
        transform: rotateZ(360deg);
      }
    }
  }

  .mdi-cog {
    font-size: 2em;
    color: rgb(202,186,156) !important;
    -webkit-transition: 0.6s ease-out;
    -moz-transition:  0.6s ease-out;
    transition:  0.6s ease-out;
  }
}

.wordBox {
  float: left;
  position: relative;
  align-self: flex-start;
  display: flex;
  height: 8em;
  width: 8em;
  background-color: #102820;
  border-radius: 1em;

  .wordChosen {
    color: rgb(202,186,156);
    transform: translateY(-50%);
    position: absolute;
    font-size: xxx-large;
    top: 50%;
    width: 2.5em;
  }
}

.credit-box {

  font-size: 1.1em;
  font-weight: 600;
  .mdi {
    top: -0.1em;
    font-size: 1.1em;
  }
}


.title-box {
    position: relative;
    width: auto;

  .title-text {
    display: contents;
    font-family: 'Josefin Sans';
    color: #102820;
    position: absolute;
    font-weight: 600;
    font-size: xxx-large;
    top: 50%;
    width: fit-content;
  }
}
.v-list-item > .v-list-item__content > .v-input--horizontal {
    grid-template-rows: max-content minmax(0, 0fr) max-content;
    margin: 0.5em 1em 1em 1em;

    .mdi-magnify-plus-outline, .mdi-magnify-minus-outline {
      color: black;
    }
}

.v-list-item {

  padding-bottom: 16px;
  .mode-text {
    font-size: 1.2em;
    font-weight: 600;
    text-align: center;
    margin: 0.5em 1em 0.5em 1em;
  }

  .v-btn {
    background-color: rgb(202,186,156) !important;
    border-color: #102820;
    color: #102820 !important;
    border-radius: 0.5em;
    border-style: solid;
    border-width: 0.25em;
    font-weight: 600;
    transition-property: border-right;
    transition:  0.3s ease-out;
  }

  :deep(.mdi) {
    color: #102820;
    opacity: 1;
  }
}

:deep(.v-slider-track__ticks) {
  font-weight: 600;
  color: #102820;
}

:deep(.v-btn--disabled.v-btn--variant-elevated .v-btn__overlay) {
  opacity: 0;
}

:deep(.v-btn--disabled.v-btn--variant-elevated) {
  border-color: #102820;
  border-radius: 0.5em;
  border-style: solid;
  border-width: 0.375em;
  border-right: 2em solid #102820;
}

.v-list {
  background-color: rgb(202,186,156) !important;
  color: #102820;
  border-color: #102820;
  border-radius: 1em 0em 1em 1em !important;
  border-style: solid;
  border-width: 0.5em;
  margin-top: 0.25em;

  .bottom-border {
    border-bottom: 0.5em solid #102820;
  }

  .mode-option {
    padding-bottom: 16px;
  }
}
</style>
