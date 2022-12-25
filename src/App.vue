<template>
  <div class="container" ref="DownloadComp">
    <Header @fetch-random-tweet="fetchRandomTweet" @go-to-previous-tweet="goToPreviousTweet"
      @go-to-next-tweet="goToNextTweet" @download-as-pdf="downloadAsPdf" title="Fetch Radom Technical Tweet"
      :tweet="tweets[0]" />
    <Tweets :tweets="tweets" />
  </div>
</template>

<script>
import Header from "./components/Header"
import Tweets from "./components/Tweets"

export default {
  name: 'App',
  components: {
    Header,
    Tweets,
  },
  data() {
    return {
      tweets: [],
      isFetching: false,
    }
  },
  props: {
    history: Array,
  },
  methods: {
    async fetchRandomTweet() {
      this.isFetching = true
      let res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/metadata")
      let results = await res.json()
      let randomNumber = Math.floor((Math.random() * results.entries) + 1);

      res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/tweets/" + randomNumber)
      results = await res.json()

      res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/tweets?tweet_conversation_id=" + results.tweet_conversation_id + "&_sort=tweet_id")
      const tweets = await res.json()

      const onlyContainsOthers = (currentValue) => currentValue.tweet_type === "Others";
      if (tweets.every(onlyContainsOthers)) {
        await this.fetchRandomTweet()
      } else {
        this.tweets = tweets
        this.visited.push(results.tweet_conversation_id)
        this.current_index = this.visited.length - 1
      }
      this.isFetching = false
    },
    async goToPreviousTweet() {
      if (this.current_index < 1) {
        console.log("Either there are no tweets or you are in the first random tweet")
        return
      }
      const tweet_conversation_id = this.visited[this.current_index - 1]
      const res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/tweets?tweet_conversation_id=" + tweet_conversation_id + "&_sort=tweet_id")
      this.tweets = await res.json()
      this.current_index -= 1
    },
    async goToNextTweet() {
      if (this.current_index + 1 === this.visited.length) {
        console.log("Either there are no tweets or you are in the last random tweet")
        return
      }
      const tweet_conversation_id = this.visited[this.current_index + 1]
      const res = await fetch("https://random-tech-tweet-generator-backend.vercel.app/tweets?tweet_conversation_id=" + tweet_conversation_id + "&_sort=tweet_id")
      this.tweets = await res.json()
      this.current_index += 1
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
    this.tweets = []
    this.visited = []
    this.current_index = 0
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
