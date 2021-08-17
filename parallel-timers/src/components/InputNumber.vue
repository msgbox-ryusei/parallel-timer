<template>
  <div>
    <v-text-field
      type="number"
      :label="label"
      v-model="inputText"
      @input="checkValue"
    ></v-text-field>
  </div>
</template>
<script>
export default {
  props: {
    value: {
      default: "",
      type: [String, Number],
    },
    label: {
      label: "",
      type: String,
    },
    max: {
      default: 99999,
      type: Number,
    },
    min: {
      default: 0,
      type: Number,
    },
  },
  data() {
    return {
      inputText: this.value,
    };
  },
  watch: {
    inputText() {
      this.$emit("input", this.inputText);
    },
  },
  methods: {
    checkValue() {
      if (this.inputText >= this.max) {
        this.inputText = this.max;
      }
      if (this.inputText <= this.min) {
        this.inputText = this.min;
      }
    },
  },
};
</script>
