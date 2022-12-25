# fetch-random-tech-tweet

> This is inspired by [Vue JS Crash Course by Traversy Media](https://github.com/bradtraversy/vue-crash-2021).
> This is currently hosted in [Vercel](https://vercel.com/). You can view the application [here](https://fetch-random-tech-tweet.vercel.app/).

## What is this?

Many developer use Twitter as source to share educational technical material which may help beginner, intermediate or expert developers.
This repository contains code for simple Vue application which can be used to fetch these technical tweets thread in random order. Each thread may belong to certain category like Python, API, Django, System Design, Linux etc. You have following buttons which perform respective actions:

- Fetch tweet: This will fetch a random tweet from the existing technical tweet source. To know how the technical tweets were fetched, categories and stored, check the [README for the backend](https://github.com/Dibakarroy1997/fetch-random-tech-tweet-backend/blob/main/README.md).
- Prev: Will show the previously fetched random technical tweet. If no random tweets were fetched before or this is the first random tweet which is featched then this does not do anything.
- Next: This only is useful if you have used 'Prev' and want to check the recent random technical tweet generated.
- Tweet Source: This will open the actual tweet in https://twitter.com/.
- Download as PDF: As the name says, this will download the current tweet as PDF.

## Demo

![](static/Demo.gif)

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
