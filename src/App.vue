<template>
  <div class="container">
    <div class="row">
        <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 form-group">
            <label for="wordlist">Word List</label><br>
            <!-- Interpolation between <textarea>{{ test }}</textarea> doesn't work!-->
            <textarea
                    id="wordlist"
                    rows="5"
                    class="form-control"
                    v-model.lazy.trim="wordlist"
                    @blur="randomWord"></textarea><br>
            <input type="checkbox" id="checkbox" v-model="checked">
            <label for="checkbox">Hide some letters?</label>
        </div>
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 form-group">

        <p @click="randomWord">{{word | hiddenLetters(checked)}}</p>
      </div>
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
      checked: false
    }
  },
  methods: {
    randomWord(){
      // let words = _.compact(this.wordlist.split('\n'));
      let words = _.without(this.wordlist.split('\n'), '');
      let word = this.word;
      while (word === this.word) {
        this.word = words[Math.floor(Math.random()*words.length)];
      }
    }
  },
  filters: {
    hiddenLetters(value, checked){
      let newValue = value;
      let sample = _.sampleSize(value.split(''), Math.ceil(value.length/3));

      sample.forEach((letter) => {
        newValue = newValue.replace(letter,'_');
      });

      return checked ? newValue : value;
    }
  },
  created(){
    this.randomWord();
  }
}
</script>

<style lang="scss">
body {
  font: 1em Arial,sans-serif;
}
p {
  font-size: 3em;
  text-transform: capitalize;
  letter-spacing: .2em;
}
p::selection {
  background-color: #fff;
}

</style>
