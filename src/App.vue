<template>
  <div class="key-board" @click="focusKeyBuffer">
    <div class="left-part">
      <div class="function-keys line-keys">
        <key
          class="key"
          v-for="item in functionKeys"
          :key="item.index"
          :keyItem="item"
          :isPress="item.isPress"
        ></key>
      </div>
      <div class="primary-keys">
        <div class="line-1 line-keys">
          <key
            class="key"
            v-for="item in primaryKeys.LINE1"
            :key="item.index"
            :keyItem="item"
            :isPress="item.isPress"
          ></key>
        </div>
        <div class="line-2 line-keys">
          <key
            class="key"
            v-for="item in primaryKeys.LINE2"
            :key="item.index"
            :keyItem="item"
            :isPress="item.isPress"
          ></key>
        </div>
        <div class="line-3 line-keys">
          <key
            class="key"
            v-for="item in primaryKeys.LINE3"
            :key="item.index"
            :keyItem="item"
            :isPress="item.isPress"
          ></key>
        </div>
        <div class="line-4 line-keys">
          <key
            class="key"
            v-for="item in primaryKeys.LINE4"
            :key="item.index"
            :keyItem="item"
            :isPress="item.isPress"
          ></key>
        </div>
        <div class="line-5 line-keys">
          <key
            class="key"
            v-for="item in primaryKeys.LINE5"
            :key="item.index"
            :keyItem="item"
            :isPress="item.isPress"
          ></key>
        </div>
      </div>
      <div class="control-keys"></div>
    </div>
    <div class="right-part">
      <div class="status-indication"></div>
      <div class="number-keys"></div>
    </div>
    <div class="key-buffer">
      <textarea
        ref="keyBuffer"
        @keydown="typingStart"
        @keypress="typing"
        @keyup="typingEnd"
      ></textarea>
    </div>
    <button class="button" @click="outJson">测试按钮</button>
  </div>
</template>

<script>
import Key from "./components/Key.vue";
import { ALL_KEYS, FUNCTION_KEYS, PRIMARY_KEYS } from "./common/config";

import { ref } from "vue";

export default {
  name: "App",
  components: {
    Key,
  },
  setup() {
    // 特殊的Shift ShiftLeft 没有 ShiftRight。。。

    const map = ALL_KEYS.reduce(
      (map, item) => map.set(item.code, item),
      new Map()
    );
    const keyMap = ref(map);
    const functionKeys = ref(FUNCTION_KEYS.map((item) => map.get(item)));
    const primaryKeys = ref({
      LINE1: PRIMARY_KEYS.LINE1.map((item) => map.get(item)),
      LINE2: PRIMARY_KEYS.LINE2.map((item) => map.get(item)),
      LINE3: PRIMARY_KEYS.LINE3.map((item) => map.get(item)),
      LINE4: PRIMARY_KEYS.LINE4.map((item) => map.get(item)),
      LINE5: PRIMARY_KEYS.LINE5.map((item) => map.get(item)),
    });
    // console.log(keyMap.size);
    return {
      functionKeys,
      primaryKeys,
      typeValue: "",
      keyMap,
      keyJson: [],

      preKeyCode: null,
      curKeyCode: "",

      isTyping: false,
    };
  },
  mounted() {
    this.start();
  },
  methods: {
    start() {
      this.focusKeyBuffer();
    },
    focusKeyBuffer() {
      this.$refs.keyBuffer.focus();
    },
    typingStart($event) {
      const key = this.keyMap.get($event.code);
      key.isPress = true;

      this.curKeyCode = key.code;

      if (this.preKeyCode !== null && this.preKeyCode !== this.curKeyCode) {
        const preKey = this.keyMap.get(this.preKeyCode);
        preKey.isPress = false;
      }

      this.preKeyCode = key.code;

      $event.preventDefault();
    },
    typing($event) {
      this.isTyping = true;

      const key = this.keyMap.get($event.code);
      key.isPress = true;

      this.curKeyCode = key.code;

      if (this.preKeyCode !== null && this.preKeyCode !== this.curKeyCode) {
        const preKey = this.keyMap.get(this.preKeyCode);
        preKey.isPress = false;
      }

      this.preKeyCode = key.code;

      $event.preventDefault();
    },
    typingEnd($event) {

      const key = this.keyMap.get($event.code);
      key.isPress = false;
      
      const preKey = this.keyMap.get(this.preKeyCode);
      preKey.isPress = false;

      this.curKeyCode = null;
      this.preKeyCode = key.code;

      $event.preventDefault();
    },
    outJson() {
      // console.log(this.keyJson);
      // const arr = this.FUNCTION_KEYS.map((item) => item.value);
      const arr = {
        LINE1: this.PRIMARY_KEYS.LINE1.map((item) => item.value),
        LINE2: this.PRIMARY_KEYS.LINE2.map((item) => item.value),
        LINE3: this.PRIMARY_KEYS.LINE3.map((item) => item.value),
        LINE4: this.PRIMARY_KEYS.LINE4.map((item) => item.value),
        LINE5: this.PRIMARY_KEYS.LINE5.map((item) => item.value),
      };
      console.log(arr);
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

.key-board {
  margin: 10vh 0;
  padding: 0 10vw;
  background: #e0e0e0;
  overflow: hidden;
}
.line-keys {
  display: flex;
  justify-content: space-between;
}
.key {
  margin: 10px;
}
</style>
