<template>
  <div class="container">
    <form v-if="!isSubmitted">
      <label for="wordlist">Word List</label><br>
      <!-- Interpolation between <textarea>{{ test }}</textarea> doesn't work!-->
      <textarea id="wordlist" rows="10" class="form-control" v-model.lazy.trim="wordlist" @blur="randomWord"></textarea>
      <br>
      <p>Hide some letters?</p>
      <label for="none">
          <input
                  type="radio"
                  id="none"
                  value="None"
                  v-model="hideLetters"
                  name="hideLetters"> None
      </label>
      <label for="random">
          <input
                  type="radio"
                  id="random"
                  value="Random"
                  v-model="hideLetters"
                  name="hideLetters"> Random
      </label>
      <label for="firstlast">
          <input
                  type="radio"
                  id="firstlast"
                  value="FirstLast"
                  v-model="hideLetters"
                  name="hideLetters"> FirstLast
      </label>
      <label for="internals">
          <input
                  type="radio"
                  id="internals"
                  value="Internals"
                  v-model="hideLetters"
                  name="hideLetters"> Internals
      </label>

      <br><br>
      <button class="btn btn-primary" @click.prevent="submitted">Start &rarr;</button>
    </form>
    <div class="word-display" v-else>
      <button class="btn btn--return" @click.prevent="submitted">&larr;</button>
      <p @click.stop="randomWord">{{word | randomLetters(hideLetters) | firstLastLetters(hideLetters) | internalLetters(hideLetters)}}</p>
    </div>
  </div>
</template>

<script>
  import _ from 'lodash';

  export default {
    data () {
      return {
        wordlist: 'one\ntwo\nthree',
        word: '',
        checked: false,
        hideLetters: 'None',
        isSubmitted: false
      }
    },
    methods: {
      randomWord(){
      let words = _.without(this.wordlist.split('\n'), '');
      let word = this.word;
      while (word === this.word) {
        this.word = words[Math.floor(Math.random()*words.length)];
      }
    },
    submitted(){
      this.isSubmitted = !this.isSubmitted;
    }
  },
  filters: {
    randomLetters(value, hideLetters){
      let newValue = value;
      let sample = _.sampleSize(value.split(''), Math.ceil(value.length/3));

      sample.forEach((letter) => {
        newValue = newValue.replace(letter,'_');
      });

      return hideLetters === 'Random' ? newValue : value;
    },
    firstLastLetters(value, hideLetters){
      if (hideLetters === 'FirstLast') {
        return `_${value.slice(1,-1)}_`;
      } else {
        return value;
      }
    },
    internalLetters(value, hideLetters){
      if (hideLetters === 'Internals') {
        let guts = value.slice(1,-1);
        let newGuts = guts.replace(/\w/g, '_');
        return `${value.charAt(0)}${newGuts}${value.slice(-1)}`;
      } else {
        return value;
      }
    }
  },
  created(){
    this.randomWord();
  }
}
</script>

<style lang="scss">
  body {
    background-color: #424242;
    color: #fff;
    font: 1em Arial,sans-serif;
  }

  .container {
    position: absolute;
    top: 0;left: 0;bottom: 0;right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .btn {
    background-color: #3299BB;
    border: none;
    border-radius: 5px;
    box-shadow: 0 0 5px #333;
    color: #fff;
    font-size: 1rem;
    font-weight: bold;
    padding: 10px;

    &--return {
      background-color: #BCBCBC;
      position: absolute;
      top: 5px;left: 5px;
    }
  }

  textarea {
    background-color: #BCBCBC;
    border-radius: 5px;
    font-size: 1rem;
    min-width: 300px;
    padding: 5px;
  }

.word-display {
  p {
    background-color: #BCBCBC;
    border: 10px solid #3299BB;
    border-radius: 10px;
    font-size: 4em;
    font-weight: bold;
    letter-spacing: .2em;
    padding: 10px 15px 10px 30px;
  }
  p::selection {
    background-color: #424242;
  }

}


</style>
