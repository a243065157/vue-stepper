<template>
  <div class="step-box-container" :style="{'width': stepWidth, 'height': stepHeight + 'px'}">
    <div class="step-btn" @click="minusNum">
      <i :class="minusIconName"></i>
    </div>
    <div class="step-input">
      <input type="text" 
        :value="stepVal" 
        @blur="inputBlur"
        @change="onChange"
        @keypress="onKeyPress"
        @keydown="onKeyDown"
        @input="inputNum"
        :readonly="readonly"
        :autofocus="autofocus"
        :disabled="disabled"
      >
    </div>
    <div class="step-btn" @click.prevent="addNum">
      <i :class="addIconName"></i>
    </div>
    </div>
</template>
<script>
  export default {
    name: 'stepBox',
    props: {
      value: {
        type: [String, Number],
        default: 0
      },
      stepWidth: {
        type: String,
        default: '100%'
      },
      stepHeight: {
        type: Number,
        default: 10
      },
      stepMax: {
        type: Number,
        default: 100
      },
      stepMin: {
        type: Number,
        default: 0
      },
      step: {
        type: Number,
        default: 1
      },
      minusIconName: {
        type: String,
        default: 'iconfont icon-minus'
      },
      addIconName: {
        type: String,
        default: 'iconfont icon-add'
      },
      readonly: {
        type: Boolean,
        default: () => {
          return false
        }
      },
      autofocus: {
        type: Boolean,
        default: () => {
          return false
        }
      },
      disabled: {
        type: Boolean,
        default: () => {
          return false
        }
      },
      intOnly: {
        // 是否只支持整数
        type: Boolean,
        default: () => {
          return false
        }
      }
    },
    data () {
      return {
        keyStatus: false,
        stepVal: this.value
      }
    },
    methods: {
      isNumber (keyCode) {  
        // 数字  
        if (keyCode >= 48 && keyCode <= 57 ) return true  
        // 小数字键盘  
        if (keyCode >= 96 && keyCode <= 105) return true  
        // Backspace键  
        if (keyCode == 8) return true  
        return false  
      },
      inputChange (e) {
        let val = e.target.value.trim()
      },
      inputNum (e) {
        let value = e.target.value
        if (value > this.stepMax) {
          value = this.stepMax
          e.target.value = value
        } else if (value < this.stepMin) {
          value = this.stepMin
          e.target.value = value
        }
        this.setVal(value)
        this.$emit('on-change', value)
      },
      inputBlur (e) {
        if  (e.target.value.length <= 0) {
          e.target.value = 0
          this.$emit('input', e.target.value)
        }
        let value = e.target.value
        this.$emit('on-blur', value)
      },
      setVal (value) {
        this.$nextTick(() => {
          // 设置值
          if (value === this.setVal) return
          this.stepVal = value
          this.$emit('input', value)
        })
      },
      onChange (value) {
      },
      onKeyPress (e) {
        return false
      },
      onKeyDown (e) {
        if (!this.intOnly) {
          return true
        }
        let keyCode = e.keyCode
        this.keyStatus = !this.isNumber(keyCode)
        if (!this.isNumber(keyCode)) event.preventDefault()
      },
      addNum () {
        // 点击加号
        if (this.disabled) return
        let value = this.stepVal
        value+= this.step
        if (value > this.stepMax) {
          value = this.stepMax
        }
        this.setVal(value)
        this.$emit('on-change', value)
      },
      minusNum () {
        // 点击加号
        if (this.disabled) return
        let value = this.stepVal
        value-= this.step
        if (value < this.stepMin) {
          value = this.stepMin
        }
        this.setVal(value)
        this.$emit('on-change', value)
      }
    },
    watch: {
      value (val) {
        this.setVal(val)
      }
    },
    mounted () {
      this.$nextTick(() => {
        this.stepVal = this.value
      })
    }
  }
</script>
<style scoped lang="less">
  .step-box-container{
    display: flex;
    border-radius: 3px;
    border: 1px solid #444d62;
    overflow: hidden;
    display: flex;
    justify-content: space-between;
    background: #2c3342;
    .step-input{
      flex: 3;
      height: 100%;
      input{
        background: transparent;
        border: 0;
        color: #fff;
        text-align: center;
        width: 100%;
        height: 100%;
        outline: none;
        display: block;
      }
    }
    .step-btn{
      flex: 2;
      color: #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 8px;
      cursor: pointer;
    }
  }
</style>
