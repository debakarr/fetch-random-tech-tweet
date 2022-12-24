<template>
  <div class="container" ref="DownloadComp">
    <Header @generate-random-tweet="generateRandomTweet" @go-to-previous-tweet="goToPreviousTweet"
      @go-to-next-tweet="goToNextTweet" @download-as-pdf="downloadAsPdf" title="Random Tech Tweet Generator"
      :tweet="tweets[0]" />
    <Tweets :tweets="tweets" />
  </div>
</template>

<script>
import Header from "./components/Header"
import Tweets from "./components/Tweets"
import VueHtml2pdf from 'vue-html2pdf'

export default {
  name: 'App',
  components: {
    Header,
    Tweets,
    VueHtml2pdf
  },
  data() {
    return {
      tweets: [],
    }
  },
  props: {
    history: Array
  },
  methods: {
    async generateRandomTweet() {
      let res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/metadata")
      let results = await res.json()
      let randomNumber = Math.floor((Math.random() * results.entries) + 1);

      res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/tweets/" + randomNumber)
      results = await res.json()

      res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/tweets?tweet_conversation_id=" + results.tweet_conversation_id + "&_sort=tweet_id")
      const tweets = await res.json()

      const onlyContainsOthers = (currentValue) => currentValue.tweet_type === "Others";
      if (tweets.every(onlyContainsOthers)) {
        await this.generateRandomTweet()
      } else {
        this.tweets = tweets
        self.visited.push(results.tweet_conversation_id)
        self.current_index = self.visited.length - 1
      }
    },
    async goToPreviousTweet() {
      if (self.current_index < 1) {
        console.log("Either there are no tweets or you are in the first random tweet")
        return
      }
      const tweet_conversation_id = self.visited[self.current_index - 1]
      const res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/tweets?tweet_conversation_id=" + tweet_conversation_id + "&_sort=tweet_id")
      this.tweets = await res.json()
      self.current_index -= 1
    },
    async goToNextTweet() {
      if (self.current_index + 1 === self.visited.length) {
        console.log("Either there are no tweets or you are in the last random tweet")
        return
      }
      const tweet_conversation_id = self.visited[self.current_index + 1]
      const res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/tweets?tweet_conversation_id=" + tweet_conversation_id + "&_sort=tweet_id")
      this.tweets = await res.json()
      self.current_index += 1
    },
    downloadAsPdf() {
      let w = window.open()
      w.document.write(this.$refs.DownloadComp.innerHTML)
      w.document.close()
      w.setTimeout(function () {
        w.print()
      }, 1000)
    }
  },
  async created() {
    self.tweets = []
    self.visited = []
    self.current_index = 0
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Poppins', sans-serif;
}

.container {
  max-width: 1270px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
