<template>
  <div :class="keyboardClass"></div>
</template>

<script>
import Keyboard from 'simple-keyboard';
import 'simple-keyboard/build/css/index.css';

export default {
  name: 'SimpleKeyboard',
  props: {
    keyboardClass: {
      default: 'simple-keyboard',
      type: String,
    },
    inputs: {
      type: Object,
    },
    inputName: {
      type: String,
    },
  },
  data: () => ({
    keyboard: null,
  }),
  mounted() {
    this.keyboard = new Keyboard({
      onChange: this.onChange,
      onKeyPress: this.onKeyPress,
      inputName: this.inputName,
      /*debug: true,
      onRender: (keyboard) => {
        console.log('Keyboard rendered', keyboard.input);
      },*/
    });
  },
  methods: {
    onChange(input) {
      this.$emit('onChange', input);
    },
    onKeyPress(button) {
      this.$emit('onKeyPress', button);

      /**
       * If you want to handle the shift and caps lock buttons
       */
      if (button === '{shift}' || button === '{lock}') this.handleShift();
    },
    handleShift() {
      let currentLayout = this.keyboard.options.layoutName;
      let shiftToggle = currentLayout === 'default' ? 'shift' : 'default';

      this.keyboard.setOptions({
        layoutName: shiftToggle,
      });
    },
  },
  watch: {
    inputName(inputName) {
      console.log(
        'SimpleKeyboard: inputName updated',
        inputName,
        this.inputs[inputName]
      );
      this.keyboard.setOptions({ inputName });
    },
    inputs: {
      handler(inputs) {
        console.log(
          'SimpleKeyboard: input Updated',
          this.keyboard.options.inputName,
          inputs
        );
        this.keyboard.replaceInput(inputs);
      },
      deep: true,
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
