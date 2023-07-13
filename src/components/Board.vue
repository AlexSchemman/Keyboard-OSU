<template>
  <v-container>
    <div class="board">
      <div class="row">
        <div v-for="(Button, key) in Rows[0]" v-bind:key="Button">
          <div
            class="button"
            :class="key"
            @mouseenter="HoverEl = Button"
            @mouseleave="HoverEl = HoverEl==Button? 'null' : Button"
            >
            <span v-if="key!=highlightedKey" class="key-text">
              {{  Button }}
            </span>
            <span v-else class="key-text colored">
              {{  Button }}
            </span>
          </div>
        </div>
      </div>
      <div class="row">
        <div v-for="(Button, key) in Rows[1]" v-bind:key="Button">
          <div
            class="button"
            :class="key"
            @mouseenter="HoverEl = Button"
            @mouseleave="HoverEl = HoverEl==Button? 'null' : Button"
            >
            <span v-if="key!=highlightedKey" class="key-text">
              {{  Button }}
            </span>
            <span v-else class="key-text colored">
              {{  Button }}
            </span>
          </div>
        </div>
      </div>
      <div class="row">
        <div v-for="(Button, key) in Rows[2]" v-bind:key="Button">
          <div
            class="button"
            :class="key"
            @mouseenter="HoverEl = Button"
            @mouseleave="HoverEl = HoverEl==Button? 'null' : Button"
            >
            <span v-if="key!=highlightedKey" class="key-text">
              {{  Button }}
            </span>
            <span v-else class="key-text colored">
              {{  Button }}
            </span>
          </div>
        </div>
      </div>
      <div class="row">
        <div v-for="(Button, key) in Rows[3]" v-bind:key="Button">
          <div
            class="button"
            :class="key"
            @mouseenter="HoverEl = Button"
            @mouseleave="HoverEl = HoverEl==Button? 'null' : Button"
            >
            <span v-if="key!=highlightedKey" class="key-text">
              {{  Button }}
            </span>
            <span v-else class="key-text colored">
              {{  Button }}
            </span>
          </div>
        </div>
      </div>
      <div class="row">
        <div v-for="(Button, key) in Rows[4]" v-bind:key="Button">
          <div
            class="button"
            :class="key"
            @mouseenter="HoverEl = Button"
            @mouseleave="HoverEl = HoverEl==Button? 'null' : Button"
            >
            <span v-if="key!=highlightedKey" class="key-text">
              {{  Button }}
            </span>
            <span v-else class="key-text colored">
              {{  Button }}
            </span>
          </div>
        </div>
      </div>
    </div>
    <div class="debug-box" v-if="debugConfig">
      <div class="debug-title" id="demo">
        Debug Box
      </div>
      <div class="debug-text">
        <div>
          Hovering on : {{ HoverEl }}
        </div>
        <div>
          Key Pressed : {{ trackedHoverEl }}
        </div>
        <div>
          Last Random Key: {{ lastRngKey }}
        </div>
      </div>
    </div>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    Chosen: {
      type: String,
      required: true,
      default: '',
    },
    settingsConfig: {
      type: Object,
      required: true,
      default: () => {},
    },
  },
  methods: {
    shuffleRows () {
      let currentIndex = this.Rows.length,  randomIndex;
      while (currentIndex != 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [this.Rows[currentIndex], this.Rows[randomIndex]] = [
          this.Rows[randomIndex], this.Rows[currentIndex]];
      }
    },
    getKeyByValue(object, value) {
      return Object.keys(object).find(key => object[key] === value);
    },
    chooseRandomHighlight() {
      let rngChoose = this.arrayOfSelectableKeys[(this.arrayOfSelectableKeys.length-1) * Math.random() | 0]
      let randomKey = rngChoose==this.Chosen || rngChoose == null?this.chooseRandomHighlight():rngChoose;
      return randomKey;
    },
  },
  created() {
    window.addEventListener('keydown', (e) => {
      const key = e.key.toUpperCase()== ' '? 'SPACE': e.key.toUpperCase();
      this.trackedHoverEl = key;
      if (this.modeConfig == 0){
        if (key == 'ESCAPE' && this.HoverEl == this.Chosen) {
          this.shuffleRows();
          this.$emit('selected');
          this.lastRngKey = this.Chosen;
        }
      }
      if (this.modeConfig == 1){
        if (key == this.Chosen && this.HoverEl == this.Chosen) {
          this.$emit('selected');
          this.lastRngKey = this.Chosen;
        }
      }
    });

    this.sizeEL = document.querySelector(':root');

    this.Rows = [
      this.Row1,
      this.Row2,
      this.Row3,
      this.Row4,
      this.Row5
    ]
  },
  watch: {
    settingsConfig (newVal) {
      this.sizeEL.style.setProperty('--baseSize', newVal['SliderSize']);
      this.modeConfig = newVal['ModeConfig'];
      this.colConfig = newVal['ColConfig'];
      this.debugConfig = newVal['DebugConfig']
      if(this.modeConfig == 1){
        this.Rows = [
          this.Row1,
          this.Row2,
          this.Row3,
          this.Row4,
          this.Row5
        ]
      }
    },
    Chosen () {
      const highlightedValue = this.colConfig==0? this.Chosen: this.chooseRandomHighlight();
      for(let i = 0; i < this.Rows.length; i++) {
        let findHighlightedKey = this.getKeyByValue(this.Rows[i], highlightedValue)
        this.highlightedKey = findHighlightedKey == undefined? this.highlightedKey :findHighlightedKey;
      }

      let element = document.getElementsByClassName(this.highlightedKey)
      element[0].classList.add("colored");
    },
  },
  data() {
    return {
      HoverEl: '',
      trackedHoverEl: '',
      sizeEL: {},
      Rows: [],
      modeConfig: false,
      colConfig: false,
      debugConfig: false,
      highlightedKey: '',
      lastRngKey: '',
      arrayOfSelectableKeys: [
        '1', '2', '3', '4', '5', '6', '7', '8', '9', '0',
        'Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P',
        'A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L',
        'Z', 'X', 'C', 'V', 'B', 'N', 'M'
      ],
      Row1: {
        symbol_esc: 'esc', 
        number_one:'1',
        number_two: '2',
        number_three : '3',
        number_four: '4',
        number_five: '5',
        number_six: '6',
        number_seven: '7',
        number_eight: '8',
        number_nine: '9',
        number_zero: '0',
        symbol_dash: '-',
        symbol_equal: '=',
        symbol_bck: 'BACKSPACE', 
      },
      Row2:  {
        symbol_tab: 'TAB',
        letter_q : 'Q',
        letter_w: 'W',
        letter_e: 'E',
        letter_r: 'R',
        letter_t: 'T',
        letter_y: 'Y',
        letter_u: 'U',
        letter_i: 'I',
        letter_o: 'O',
        letter_p: 'P',
        symbol_openSqBrckt: '[',
        symbol_closeSqBrckt: ']',
        symbol_bslash: '\\'
      },
      Row3: {
        symbol_caps: "CAPS",
        letter_a : 'A', 
        letter_s: 'S', 
        letter_d: 'D', 
        letter_f: 'F', 
        letter_g: 'G', 
        letter_h: 'H', 
        letter_j: 'J', 
        letter_k: 'K', 
        letter_l: 'L', 
        symbol_semicolon: ';', 
        symbol_singleQuote: "'",
        symbol_enter: "ENTER"
      },
      Row4: {
        symbol_rshift: "SHIFT",
        letter_z: 'Z', 
        letter_x: 'X', 
        letter_c: 'C', 
        letter_v: 'V', 
        letter_b: 'B', 
        letter_n: 'N', 
        letter_m: 'M', 
        symbol_comma: ',', 
        symbol_fullStop: '.', 
        symbol_fslash: '/',
        symbol_lshift: "SHIFT"
      },
      Row5: {
        symbol_lctrl: "CTRL",
        symbol_win: "WIN",
        symbol_lalt: "ALT",
        symbol_space: "SPACE",
        symbol_ralt: "ALT",
        symbol_fn: "",
        symbol_rctrl: "CTRL"
      },
    }
  }
}
</script>

<style scoped lang="scss">
.v-container {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;

  .board {
    display: block;

    .row {
      display: flex;

      :deep(.button) {
        font-size: calc((var(--baseSize)/4)*1);
        margin: calc((var(--baseSize)/6)/3);
        height: var(--baseSize) !important;
        width: var(--baseSize);
        min-width: var(--baseSize);
        min-height: var(--baseSize) !important;
        border-radius: calc((var(--baseSize)/3)*0.75);

        color: rgb(202,186,156);
        align-items: center;
        display: inline-grid;
        justify-content: center;
        letter-spacing: 0.0892857143em;
        text-indent: 0.0892857143em;
        line-height: normal;
        text-transform: uppercase;
        flex-shrink: 0;
        border-color: rgb(202,186,156);
        border-style: solid;
        border-width: calc((var(--baseSize)/3)*0.25);

        .key-text {
          font-size:calc((var(--baseSize)/3)*0.775);
          font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
          font-weight: 500;
          color: inherit;
        }
      }

      :deep(.button:has(.colored))  {
        background-color: rgb(202,186,156) !important;
        color: #102820;
        -webkit-text-stroke-width: 1px;
        -webkit-text-stroke-color: #102820;
      }

      :deep(.symbol_bck){
        width: calc((var(--baseSize)/3)*7);
      }

      :deep(.symbol_tab) {
        width: calc((var(--baseSize)/3)*5);
      }
      
      :deep(.symbol_bslash) {
        width: calc((var(--baseSize)/3)*5);
      }

      :deep(.symbol_caps) {
        width: calc((var(--baseSize)/3)*6);
      }

      :deep(.symbol_enter) {
        width: calc((var(--baseSize)/3)*7.375);
      }

      :deep(.symbol_rshift) {
        width: calc((var(--baseSize)/3)*8);
      }

      :deep(.symbol_lshift) {
        width: calc((var(--baseSize)/3)*8.75);
      }

      :deep(.symbol_space) {
        width: calc((var(--baseSize)/3)*21.425) !important;
      }

      :deep(.symbol_lctrl),:deep(.symbol_win), :deep(.symbol_lalt),
      :deep(.symbol_ralt),:deep(.symbol_fn), :deep(.symbol_rctrl) {
        width: calc((var(--baseSize)/3)*4.5) !important;
      }
    } 
  }

  .debug-box {
    color: #102820;
    background-color: rgb(202,186,156);
    border-radius: 0em 1em 0em 0em;
    border-color: rgb(202,186,156);
    border-style: solid;
    border-width: 0.25em;
    width: 18%; 
    bottom: 0%;
    left: 0%;
    position: fixed;
    font-family: monospace;

    .debug-title {
      text-align: left;
      border-radius: 0em 0.375em 0em 0em;
      background-color: #102820;
      color: rgb(202,186,156);
      font-size: 2em;
      padding-left: 0.45em;
      border-bottom: 0.25em solid rgb(202,186,156);
    }

    .debug-text {
      text-align: left;
      background-color: rgb(202,186,156);
      color: #102820;
      font-size: 1.1em;
      font-weight: 600;
      margin-left: 0.7em;
    }
  }
}
</style>
