<template>
  <div>
    <math-field ref="mathField" @click="toggleVirtualKeyboard" :value="value" @input="onInput" />
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import { MathfieldElement } from "mathlive";

export default {
  props: {
    value: {
      type: String,
      default: null,
    },
  },
  mounted() {
    this.$refs.mathField.focus();
  },
  emits: ["input"],
  setup(props, { emit }) {
    let value = ref("");
    const mathField = ref(null);

    const onInput = (event) => {
      value = event.target.value;
      emit("input", value);
    };

    const toggleVirtualKeyboard = () => {
      if (mathField.value) {
        mathField.value.executeCommand("toggleVirtualKeyboard");
      }
    };

    function handleChange(value) {
      emit("input", value);
    }

    onMounted(() => {
      mathField.value = new MathfieldElement({
        mathVirtualKeyboardPolicy: "auto",
      });
    });

    return {
      handleChange,
      mathField,
      onInput,
      toggleVirtualKeyboard,
    };
  },
};
</script>