<template>
  <div class="container">
    <div class="buttons btn-group">
      <a @click="fetchRandomQuote" class="btn btn-lg">Następny cytat</a>
      <a 
        class="btn btn-lg"
        v-bind:href="url"
        data-size="large"
      >
        <i class="fa fa-twitter"></i>Udostępnij
      </a>
    </div>
    <div class="row justify-content-center">
      <div class="quote col-md-7">
        <blockquote />
        <p class="qoute__author">{{ quote.author }}</p>
      </div>
    </div>
  </div> 
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        quote: {},
        url: ''
      }
    },
    methods: {
      fetchRandomQuote() {
        fetch('https://cytaty.herokuapp.com/api/quotes/random')
          .then(res => res.json())
          .then(json => this.quote = json[0])
          .then(() => {
            this.formatText(this.quote.text);
            this.url = `https://twitter.com/intent/tweet?text=${this.quote.text}`
          })
          .catch(error => console.log(error))
      },
      formatText(quote) {
        const words = quote.split(' ');
        let array = [];
        let wordsPerPhrase = 0;
        let markup = '';
        if (quote.length <= 50) {
          wordsPerPhrase = 2;
        } else if (quote.length <= 120) {
          wordsPerPhrase = 3;
        } else if (quote.length <= 200) {
          wordsPerPhrase = 4;
        } else {
          wordsPerPhrase = 5;
        }

        for (let i = 0; i < words.length; i += wordsPerPhrase) {
          array.push(words.slice(i, i + wordsPerPhrase).join(' '));
        }
        array.forEach((word, i) => {
          markup += `<div class="line-${i}">${word}</div>`;
        });
        document.querySelector('blockquote').innerHTML = markup;
      }
    },
    created() {
      this.fetchRandomQuote();
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Amatic+SC|Lobster|Atma|Life+Savers|Bungee');
  body {
    font-family: 'Life Savers';
    padding-top: 20px;
    color: #333;
  }
  .buttons {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    cursor: pointer;
  }
  .buttons a {
    color: #333;
    font-weight: bold;
    border: 2px solid #333;
  }
  i.fa {
    margin-right: 5px;
  }
  blockquote {
    font-size: 1.6rem;
    letter-spacing: 1.8px;
    text-align: center;
    position: relative;
    padding: 5px;
  }
  .quote {
    padding: 20px;
    margin-top: 70px;
  }
  blockquote:before {
    content: '„';
    position: absolute;
    top: -130px;
    left: -10px;
    font-size: 8rem;
  }
  blockquote:after {
    content: '”';
    position: absolute;
    bottom: -140px;
    right: -10px;
    font-size: 8rem;
  }
  p.qoute__author {
    font-size: 1.4rem;
    text-align: center;
    font-style: italic;
    font-weight: bold;
    margin-top: 40px;
  }

  blockquote div:nth-child(5n+1) {
    font-family: 'Lobster';
    font-size: 7vmin;
  }  
  blockquote div:nth-child(5n+2) {
    font-family: 'Amatic SC';
    font-size: 10vmin;
  }
  blockquote div:nth-child(5n+3) {
    font-family: 'Bungee';
    font-size: 5vmin;
  }
  blockquote div:nth-child(5n+4) {
    font-family: 'Life Savers';
    font-size: 6vmin;
    text-transform: uppercase;
  }
  blockquote div:nth-child(5n+5) {
    font-family: 'Atma';
    font-size: 7vmin;
  }
</style>
