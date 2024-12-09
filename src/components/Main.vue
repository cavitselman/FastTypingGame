<template>
  <div class="container jumbotron">
    <h1 class="display-4" style="text-align: center;">Hızlı Yazma Yarışması</h1>
    <p class="lead" style="text-align: center;">Ne kadar hızlı klavye kullandığını test et.</p>
    <hr class="my-4">

    <div v-if="isFinish" class="alert alert-primary d-flex flex-column justify-content-center align-items-center full-screen">
      <h3>Süre Bitti</h3>
      <p class="display-4">{{ dks }} DKS</p><br />
      <span>Doğruluk Yüzdesi: %{{ truePercent }} </span><br />
      <span>Doğru Kelime: {{ trueCount }} </span><br />
      <span>Yanlış Kelime: {{ falseCount }} </span>
      <button @click="newGame" class="btn btn-success mt-5 btn-lg btn-block">YENİ OYUN</button>
    </div>

    <div v-else>
      <div class="card">
        <div class="card-body">
          <span v-for="(word, key) in words.filter((data, index) => index < 20)"
                :key="key"
                :class="key === 0 ? writingWordControl : ''"
                class="words ml-2">
            {{ word }}
          </span>
        </div>
      </div>

      <div class="card">
        <div class="card-body bg-secondary">
          <div class="input-group input-group-lg justify-content-center">
            <input type="text" class="form-control" v-model="writingWord">
            <div class="input-group-append" id="button-addon4">
              <button class="btn btn-light" disabled type="button">{{ timer }} sn.</button>
              <button :disabled="isRunning" class="btn btn-light" type="button" @click="getWords">Yenile</button>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>

  import wordList from '@/assets/words.json'

  export default {
    data() {
      return {
        words: [],
        writingWord: null,
        isTrue: true,
        trueCount: 0,
        falseCount: 0,
        timer: 60,
        interval: false,
        isRunning: false,
        isFinish: false,
        wordList: wordList
      }
    },
    watch: {
      writingWord(val) {
        if (!val || val === ' ') {
          this.writingWord = ''
          return
        }
        if (!this.isRunning) this.toggleTimer()
        const word = this.words[0].slice(0, val.length).toUpperCase()
        const userWord = val.replace(' ', '').toUpperCase()
        this.isTrue = word === userWord

        if (val.indexOf(' ') !== -1) {
          this.isTrue ? this.trueCount++ : this.falseCount++
          this.words.splice(0, 1)
          this.writingWord = ''
          this.isTrue = true
        }
      }
    },
    computed: {
      writingWordControl() {
        return this.isTrue ? 'writing-word' : 'writing-word bg-danger'
      },
      dks() {
        const totalWords = 300; // Kelime sayısı burada sabit
        const remainingWords = this.words.length; // Kalan kelimeler
        const timeElapsed = 60 - this.timer; // Geçen süre
        return totalWords - remainingWords; // Kalan kelimeleri çıktı olarak döndür
      },
      truePercent() {
        const totalWords = this.trueCount + this.falseCount;
        if (totalWords === 0) return 0; // Eğer hiç kelime girilmemişse, doğruluk yüzdesi 0
        return Math.round((this.trueCount / totalWords) * 100); // Yüzdeyi virgülsüz olarak döndür
      }
    },
    mounted() {
      this.getWords()
    },
    methods: {
      newGame() {
        this.writingWord = ''; // Önceki kelimeyi temizle
        this.words = []; // Eski kelimeleri sıfırla
        this.getWords(); // Yeni kelimeleri al
        this.isFinish = false; // Oyun bitişi durumunu sıfırla
        this.timer = 60; // Süreyi sıfırla
        this.trueCount = 0; // Doğru kelime sayısını sıfırla
        this.falseCount = 0; // Yanlış kelime sayısını sıfırla
        this.isTrue = true; // Doğruluk durumunu sıfırla
        this.isRunning = false; // Timer'ı sıfırla
      },
      getWords() {
        this.words = this.wordList.sort(() => Math.random() - 0.5).splice(0, 300)
      },
      toggleTimer() {
        this.isRunning = true
        this.interval = setInterval(this.timeProcess, 1000)
      },
      timeProcess() {
        if (this.timer === 0) {
          clearInterval(this.interval)
          this.isFinish = true
          return
        }
        this.timer--
      }
    }

  }
</script>

<style>
  .words {
    font-size: 25px;
    font-weight: 400;
    display: inline-block;
    margin-right: 10px;
  }

  .writing-word {
    background-color: slategrey;
    color: white;
    padding: 5px;
    border-radius: 5px;
  }

  .input-group {
    display: flex;
    justify-content: center; /* Yatayda ortalama */
    align-items: center; /* Dikeyde ortalama */
  }

  .input-group-append {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .full-screen {
    height: 100vh; /* Ekranın tamamını kapsar */
    display: flex;
    flex-direction: column; /* İçeriği dikeyde hizalar */
    justify-content: center; /* Dikeyde tam ortalama */
    align-items: center; /* Yatayda tam ortalama */
  }

  .alert-primary {
    display: flex;
    flex-direction: column; /* İçeriği dikeyde hizalar */
    justify-content: center;
    align-items: center;
    text-align: center; /* Metni ortalar */
    height: 100%; /* Ekranı tam olarak kaplar */
  }

  button {
    width: 100%; /* Butonu tam genişlikte yapar */
  }
</style>
