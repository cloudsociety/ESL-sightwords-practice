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
                    v-model.lazy="wordlist"></textarea>
        </div>
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 form-group">
        <p v-for="word in wordsArray">{{word | hiddenLetters}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  data () {
    return {
      wordlist: 'one\ntwo\nthree'
    }
  },
  computed: {
    wordsArray(){
      return this.wordlist.split('\n');
    }
  },
  filters: {
    hiddenLetters(value){
      let newValue = value;
      let wordLength = value.length;
      // let maxSelections = Math.ceil(Math.random() * wordLength/3);
      let maxSelections = Math.ceil(wordLength/3);
      let sample = _.sampleSize(value.split(''), maxSelections);
      sample.forEach((letter) => {
        newValue = newValue.replace(letter,'_');
      });
      return `${value}: ${newValue}`;
    }
  }
}
</script>

<style lang="scss">


</style>
