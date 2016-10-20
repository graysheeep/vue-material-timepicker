<template>
  <div>
    <input placeholder="please select time" @focus="focusHandler"/>

    <transition name="movedown-fade">
      <div 
        class="timepicker"
        v-if="showPicker">
          <div class="clock-wrap">
            <div class="clock">

              <div class="timedisplay">
                <div class="innerdisplay">
                  <div class="time-left"></div>
                  <div class="showtime">
                    <span 
                      class="hour" 
                      @click="mode = 'hour'" 
                      :style="{ opacity: mode === 'hour' ? 1 : 0.7 }">{{fixHour(hour)}}</span>
                    <span>:</span>
                    <span 
                      class="minutes" 
                      @click="mode = 'minutes'"
                      :style="{ opacity: mode === 'minutes' ? 1 : 0.7 }">{{fixMinutes(minutes)}}</span>
                  </div>
                  <div class="time-right"></div>
                </div>
              </div>
              
              <div class="clockpanel">
                <div class="clockbg"></div>
                <div class="hourpicker">
                  <div 
                    class="selector"
                    :style="selectorRotateAngle"></div>
                  <span 
                    class="hourtxt" 
                    v-for="i in 12"
                    :style="getHourStyle(i % 12)"
                    @click="mode === 'hour' ? hour = i : minutes = 5 * i">{{mode === 'hour' ? i : (5 * i) % 60 || '00'}}</span>
                </div>
              </div>

              <div class="actionbuttons">
                <button>
                  <div>
                    <span>CANCEL</span>
                  </div>
                </button>
                <button>
                  <div>
                    <span>OK</span>
                  </div>
                </button>
              </div>
            </div>
          </div>
      </div>
    </transition>
      

    <div 
      class="mask" 
      @click="showPicker=false"
      :style="{ opacity: showPicker ? 1 : 0, left: showPicker ? 0 : '-100%'}"></div>
  </div>
</template>

<script>
const positions = [
  [0, 5],
  [54.5, 16.6],
  [94.4, 59.5],
  [109, 114],
  [94.4, 168.5],
  [54.5, 208.4],
  [0, 223],
  [-54.5, 208.4],
  [-94.4, 168.5],
  [-109, 114],
  [-94.4, 59.5],
  [-54.5, 19.6],
]

export default {
  name: 'hello',
  data () {
    return {
      showPicker: false,
      mode: 'hour',
      hour: 12,
      minutes: 0
    }
  },
  computed: {
    selectorRotateAngle () {
      if (this.mode === 'hour') {
        return {
          transform: `rotateZ(${this.hour * 30 + 'deg'})`
        }
      } else {
        return {
          transform: `rotateZ(${this.minutes * 6 + 'deg'})`
        }
      }
    }
  },
  methods: {
    focusHandler (e) {
      e.target.blur()
      this.showPicker = true
    },

    getHourStyle (i) {
      const hasSelected = (this.mode === 'hour' &&  this.hour % 12 === i) 
        || (this.mode === 'minutes' && this.minutes % 60 === i * 5)
      const styleObj = {
        transform: `translate(${positions[i][0] + 'px'}, ${positions[i][1] + 'px'})`,
        background: hasSelected ? 'rgb(0, 188, 212)' : 'rgba(255, 255, 255, 0)',
        color: !hasSelected ? '#2c3e50' : '#FFF'
      }
      return styleObj
    },

    fixHour (h) {
      return h < 10 ? '0' + h : h
    },

    fixMinutes (m) {
      return m % 60 < 10 ? '0' + m % 60 : m % 60
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@font-face {
  font-family: Roboto;
  src: url(../assets/Roboto-Medium.ttf);
}

* {
  font-family: Roboto;
}

input {
  outline: none;
}
.mask {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1400;
  will-change: opacity;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.541176);
  transition: opacity 400ms cubic-bezier(0.23, 1, 0.32, 1) 0ms;
}

.movedown-fade-enter-active, .movedown-fade-leave-active {
  transition: all .5s ease;
}

.movedown-fade-enter, .movedown-fade-leave {
  transform: translate3d(0, -20px, 0);
  opacity: 0;
}

.timepicker {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1500;
  width: 100%;
  height: 100%;
  padding-top: 200px;
}

.clock-wrap {
  width: 280px;
  max-width: 768px;
  border-radius: 2px;
  margin: 0 auto;
  background-color: rgb(255, 255, 255)
}

.clock {
  box-shadow: rgba(0, 0, 0, 0.247059) 0px 14px 45px, rgba(0, 0, 0, 0.219608) 0px 10px 18px;
  border-radius: 2px;
}

.timedisplay {
  padding: 14px 0;
  border-top-left-radius: 2px;
  border-top-right-radius: 2px;
  background-color: rgb(0, 188, 212);
  color: white;
}

.innerdisplay {
  margin: 6px 0px;
  line-height: 58px;
  height: 58px;
  font-size: 58px;
  display: flex;
  justify-content: center;
  align-items: baseline;
}

.time-left, .time-right {
  flex: 1 1 0%;
  position: relative;
  line-height: 17px;
  height: 17px;
  font-size: 17px;
}

.showtime {
  margin: 0px 10px;
}

.hour, .minutes {
  cursor: pointer;
}

.clockpanel {
  height: 280px;
  padding: 10px;
  position: relative;
  box-sizing: content-box;
}

.clockbg {
  position: absolute;
  top: 20px;
  width: 260px;
  height: 260px;
  border-radius: 100%;
  background-color: rgba(0, 0, 0, 0.0666667);
}

.hourpicker {
  height: 100%;
  width: 100%;
  border-radius: 100%;
  position: relative;
  /*pointer-events: none;*/
  box-sizing: border-box;
}

.hourtxt {
  display: inline-block;
  position: absolute;
  width: 32px;
  height: 28px;
  border-radius: 100%;
  left: calc(50% - 16px);
  top: 10px;
  text-align: center;
  padding-top: 5px;
  font-size: 1.1em;
  cursor: pointer;
  /*pointer-events: none;*/
}

.selector {
  height: 40%;
  background: rgb(0, 188, 212);
  width: 2px;
  left: calc(50% - 1px);
  position: absolute;
  bottom: 50%;
  transform-origin: center bottom 0px;
  pointer-events: none;
}


.actionbuttons {
  padding: 8px;
  width: 100%;
  text-align: right;
  margin-top: 0px;
  border-top: none;
}

.actionbuttons button {
  border: 10px;
  box-sizing: border-box;
  display: inline-block;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  cursor: pointer;
  text-decoration: none;
  margin: 0px;
  padding: 0px;
  outline: none;
  font-size: inherit;
  font-weight: inherit;
  transform: translate(0px, 0px);
  height: 36px;
  line-height: 36px;
  min-width: 88px;
  color: rgb(0, 188, 212);
  transition: all 450ms cubic-bezier(0.23, 1, 0.32, 1) 0ms;
  border-radius: 2px;
  position: relative;
  overflow: hidden;
  background-color: rgba(0, 0, 0, 0);
  text-align: center;
  -webkit-user-select: none;
}

.actionbuttons span {
  position: relative;
  padding-left: 16px;
  padding-right: 16px;
  vertical-align: middle;
  letter-spacing: 0px;
  text-transform: uppercase;
  font-weight: 500;
  font-size: 14px;
}
</style>
