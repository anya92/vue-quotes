<template>
  <div>
    {{ title }}
    {{ quote[0].text }}
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        title: 'Random Quote Machine',
        quote: []
      }
    },
    methods: {
      fetchRandomQuote() {
        fetch('https://cytaty.herokuapp.com/api/quotes/random')
          .then(res => res.json())
          .then(json => this.quote = json)
          .catch(error => console.log(error))
      }
    },
    created() {
      this.fetchRandomQuote();
    }
  }
</script>
