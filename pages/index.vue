<template>
  <section v-bind:class="{ 'has-text': anyText }">
    <textarea
      class="input original"
      v-model="textInput"
      ref="mainInput"
      placeholder="type here..."
    ></textarea>
    <textarea
      class="input result"
      v-model="textResult"
      @keydown.prevent=''
      @focus="$event.target.select();"
      @click="$event.target.select();"
    ></textarea>
  </section>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      textInput: ""
    };
  },
  computed: {
    textResult() {
      let wideText = "";
      this.textResultParts.forEach(word => {
        word = this.trimAndBiggen(word);
        wideText += this.widenWord(word);
        wideText += "\n";
      });
      return wideText;
    },
    textResultParts() {
      return this.textInput.replace(/\n/g, " ").split(" ");
    },
    anyText() {
      return this.textInput.length >= 1;
    }
  },
  mounted() {
    this.$refs.mainInput.focus();
  },
  methods: {
    trimAndBiggen(word) {
      return word.trim().toUpperCase();
    },
    widenWord(word) {
      let wideWord = "";
      for (let i = 0; i < word.length; i++) {
        let char = word[i];
        let isLastChar = i + 1 === word.length;
        wideWord += char;
        if (!isLastChar) {
          wideWord += " ";
        }
      }
      return wideWord;
    }
  }
};
</script>

<style>
section {
  font-size: 0;
}

.input {
  width: 100vw;
  height: 100vh;
  background-color: black;
  color: white;
  padding: 1em;
  font-size: 40px;
  transition: height 0.4s ease;
  outline: none;
  font-family: "Comic Sans MS", "MarkerFelt-Thin";
  border: none;
  border-radius: 0;
}
.result {
  height: 0;
  font-size: 0;
  overflow: hidden;
  background-color: white;
  color: black;
}
.has-text .original {
  height: 50vh;
}
.has-text .result {
  height: 50vh;
  font-size: 40px;
}
</style>
