<template>
  <div id="app">
    <div v-for="key in Object.keys(inputs)" :key="key">
      <Input
        :inputs="inputs"
        :inputName="key"
        @onInputFocus="onInputFocus"
        @onInputChange="onInputChange"
      />
    </div>

    <SimpleKeyboard
      @onChange="onChange"
      @onKeyPress="onKeyPress"
      :inputs="inputs"
      :inputName="inputName"
    />
  </div>
</template>

<script>
import SimpleKeyboard from './SimpleKeyboard';
import Input from './Input';
import './App.css';

export default {
  name: 'App',
  components: {
    SimpleKeyboard,
    Input,
  },
  data: () => ({
    /**
     * We define the inputs here
     */
    inputs: {
      input1: '',
      input2: '',
    },
    inputName: 'input1',
  }),
  methods: {
    onChange(input) {
      this.inputs[this.inputName] = input;
      console.log('onChange', this.inputs);
    },
    onKeyPress(button) {
      console.log('button', button);
    },
    onInputChange(input) {
      console.log('Input changed directly:', input.target.id);
      this.inputs[input.target.id] = input.target.value;
    },
    onInputFocus(input) {
      console.log('Focused input:', input.target.id);
      this.inputName = input.target.id;
    },
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
