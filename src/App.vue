<template>
  <div id="app">
    <div class="quote">
        <div class="quote-text">{{ quote }}</div>
        <div class="quote-author">{{ author }}</div>
    </div>
    <div class="footer">
      <div v-if="loading">Loading...</div>
      <a v-else @click="getQuote">New quote</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      quote: '',
      author: '',
      colors: [
        '#16a085',
        '#27ae60',
        '#2c3e50',
        '#f39c12',
        '#e74c3c',
        '#9b59b6',
        '#FB6964',
        '#342224',
        '#472E32',
        '#BDBB99',
        '#77B1A9',
        '#73A857',
      ],
      color: '#16a085',
      loading: false,
    };
  },
  async created() {
    await this.getQuote();
  },
  methods: {
    getRandomColor() {
      const colors = this.colors.filter((color) => color !== this.color);
      return colors[Math.floor(Math.random() * colors.length)];
    },
    async getQuote() {
      if (this.loading) return;
      this.loading = true;
      this.quote = '';
      this.author = '';

      const headers = {
        method: 'GET',
        headers: {
          'X-Api-Key': process.env.VUE_APP_API_NINJAS_KEY,
        },
      };
      const response = await fetch('https://api.api-ninjas.com/v1/quotes?category=happiness', headers);
      const data = await response.json();
      if (!data.status === 200) return;

      const { quote, author } = data[0];

      // eslint-disable-next-line no-undef
      $('.quote-text').animate({
        opacity: 0,
      }, 500, () => {
        this.quote = quote;
        // eslint-disable-next-line no-undef
        $('.quote-text').animate({
          opacity: 1,
        }, 500);
      });

      // eslint-disable-next-line no-undef
      $('.quote-author').animate({
        opacity: 0,
      }, 500, () => {
        this.author = author;
        // eslint-disable-next-line no-undef
        $('.quote-author').animate({
          opacity: 1,
        }, 500);
      });

      this.color = this.getRandomColor();

      // eslint-disable-next-line no-undef
      $('html body').animate({
        backgroundColor: this.color,
        color: this.color,
      }, 1000);

      this.loading = false;
    },
  },
};
</script>

<style lang="scss">
@import url(https://fonts.googleapis.com/css?family=Raleway:400,500);

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: "Raleway", sans-serif;
  font-weight: 400;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: #2c3e50;
}

.quote {
  border-radius: 3px;
  position: relative;
  width: 450px;
  padding: 40px 50px;
  display: table;
  background-color: #fff;
  height: auto;

  &-author {
    width: 450px;
    clear: both;
    padding-top: 20px;
    font-size: 1em;
    text-align: right;
  }

  &-text {
    text-align: center;
    width: 450px;
    clear: both;
    font-weight: 500;
    font-size: 1.75em;
  }
}

.footer {
  width: 450px;
  text-align: center;
  display: block;
  margin: 15px auto 30px auto;
  font-size: 0.8em;
  color: #fff;

  a {
    padding: 1em 2em;
    font-weight: 500;
    text-decoration: none;
    cursor: pointer;
  }
}
</style>
