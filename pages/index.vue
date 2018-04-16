<template>
  <div id="app">
    <h1 id="topic">Simple GIF Searcher</h1>

    <div class="input-area">
    <p class="text">Amount of GIFs you want to see (max 10)</p>

    
    <input class="input-box" id="quantityNumber" type="number" name="quantity" min="1" max="10" v-model="gifQuantity">
    

    <p class="text">Enter your search word below</p>
    <input class="input-box" v-model="searchTerm" type="text" v-on:keyup.enter="getGifs()">

    </div>

    <button class="button" @click="getGifs()">Search</button>

    <div class="gif-container">
      <img v-for="gif in gifs" :src="gif" :key="gif.id"> <!-- For loop through gif array, displaying each gif on the page using the :src attribute -->
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      gifQuantity: 5,
      searchTerm: "",
      gifs: []
    };
  },
  methods: {
    getGifs() {
      console.log(this.searchTerm);
      
      let apiKey = "dc6zaTOxFJmzC"; // using public Giphy API key
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?"; // endpoint for gif search
      // let limit = 5; // limiting to 5 gifs

      let limit = this.gifQuantity;

        if (limit == 69) {
        alert('Soooooooooooooooooo funny! Get out. Limiting your GIFs to the max.');
        limit = 10;
      } else if (limit > 10) {
        alert('Maximum amount is 10, limiting to the max amount. Seeing too many GIFs on one page is harmful for your mental health!');
        limit = 10;
      }

      // Using ES6 template to join variables which were set above to the url variable

      let url = `${searchEndPoint}&api_key=${apiKey}&q=${
        this.searchTerm
      }&limit=${limit}`;
      
      // Fetching using the URL string. Fetch returns a promise, this is called with then(). Returning the response as json. In case of an error, we catch it.

      fetch(url)
        .then(response => {
          return response.json();
        })
        .then(json => {
          this.buildGifs(json);
        })
        .catch(err => {
          console.log(err);
        });
    },
    // Taking json from API and entering the data part. Map each returned gif to its id. Then take the id and plug it into a url.

    buildGifs(json) {
      this.gifs = json.data
      .map(gif => gif.id)
      .map(gifId => {
        return `https://media.giphy.com/media/${gifId}/giphy.gif`;
      });
    },
      getUrl() {
        const images = document.querySelectorAll('img');

        images.forEach(img => {
          img.addEventListener('click', () => {
            var imgSource = img.src;
            alert(imgSource);
            console.log("Getting img source");
            // TODO: Add copy functionality to this, execCommand copy or vue clipboard?
          });
        });
      },
      combineGet() {
        // combining getGifs and getUrl functions so they can be called at the same time
        // TODO: Fix getUrl so it works straight away, now it basically needs a doubleclick.
        this.getGifs();
        this.getUrl();
      },
  }
};
</script>

<style>

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#topic {
  margin-bottom: 40px;
}

.text {
  margin: 20px 0px;
}

.input-area {
  margin: 10px 10px;
}
.input-box {
  padding-top: 5px;
  margin-bottom: 20px;
}

.button {
  background-color: rgb(129, 216, 208);
  font-size: 1.2em;
  color: #fff;
  padding: 10px 25px;
  border: none;
  border-radius: 3px;
  display: block;
  margin: 0 auto;
}

.button:hover {
  background-color: rgb(129, 216, 208, 0.8);
}

 /* Styling the gif portion using flexbox */

.gif-container {
  margin-top: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.gif-container > img {
  margin: 20px 5px;
}
</style>