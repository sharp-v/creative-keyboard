<template>
  <div
    class="key"
    :class="{ 'press-down': isPress }"
    :style="{ minWidth: (keyItem.width ? keyItem.width : width) + 'px' }"
  >
    <span class="value-wrapper">
      {{ keyItem.value }}
    </span>
  </div>
</template>

<script>
import { watch } from "@vue/runtime-core";
import { ARRANGEMENT } from "../common/config";
const DEFAULT_KEY = {
  width: 60,
};
export default {
  name: "key",
  props: {
    // 键入的按键对象
    keyItem: {
      type: Object,
      default() {
        return {};
      },
    },
    //
    shiftValue: {
      type: String,
      default: "",
    },
    direction: {
      type: String,
      default: ARRANGEMENT.CENTER,
    },
    width: {
      type: Number,
      default: DEFAULT_KEY.width,
    },
    isPress: {
      type: Boolean,
      default: false,
    },
  },
  updated() {
    console.log("update ", this.$props.isPress);
  },
  setup(props) {
    watch(
      () => props.isPress,
      (val, oldVal) => {
        console.log("watch ", val, oldVal);
      }
    );
    return {};
  },
};
</script>

<style>
.key {
  cursor: pointer;
  height: 60px;
  padding: 5px;

  display: flex;
  justify-content: center;
  align-items: center;

  border-radius: 10px;
  background: linear-gradient(145deg, #f0f0f0, #cacaca);
  box-shadow: 7px 7px 12px #bcbcbc, -7px -7px 12px #ffffff;

  transition: background 10s;
}

.key.press-down {
  border-radius: 10px;
  background: linear-gradient(145deg, #cacaca, #f0f0f0);
  box-shadow: 7px 7px 12px #c3c3c3, -7px -7px 12px #fdfdfd;
}

.key > span::selection {
  background: transparent;
}
</style>
