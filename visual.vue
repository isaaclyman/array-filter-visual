<template>
  <div>
    <div class="words">
      <div class="word" v-for="(word, index) in words" :key="index">
        <input type="checkbox" v-model="word.checked">
        {{ word.text }}
      </div>
    </div>
    <div class="action">
      <button @click="copyArray">Copy array</button>
    </div>
    <div class="confirm" :class="{ 'hide': !copied }">
      Copied!
    </div>
    <div class="result">
      <pre>{{wordArray}}</pre>
    </div>
  </div>
</template>

<script>
import copy from 'copy-to-clipboard'
import words from './words'

export default {
  computed: {
    wordArray () {
      const filtered = this.words.filter(word => word.checked).map(({ text }) => text)
      const str = JSON.stringify(filtered)
      const sQuote = str.replace(/"/g, "'")
      return sQuote
    }
  },
  data () {
    return {
      copied: false,
      words: words.map(text => ({
        checked: true,
        text
      }))
    }
  },
  methods: {
    copyArray () {
      copy(this.wordArray)
      this.copied = true
      setTimeout(() => {
        this.copied = false
      }, 3000)
    }
  }
}
</script>

<style>
pre {
  margin: 0;
  white-space: pre-wrap;
  word-wrap: break-word;
}

.words {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.word {
  border-left: 1px solid #CCC;
  border-right: 1px solid #CCC;
  display: flex;
  flex-direction: row;
  margin: 0;
  padding: 4px;
  text-overflow: ellipsis;
  width: 150px;
}

.action {
  margin-top: 16px;
}

.confirm {
  opacity: 1;
  transition: opacity 200ms;
}

.confirm.hide {
  opacity: 0;
}
</style>
