<template>
  <div class="list">
  <article v-for="(pokemon, index) in pokemons"
    :key="'poke'+index"
    @click="setPokemonUrl(pokemon.url)">
      <img :src="imageUrl + pokemon.id + '.png'" width="120" height="120" alt="">
      <h3 class="pokepoke">{{ pokemon.name }}</h3>
      <hr>
      <h4 id="pokeball_text">More info? Click in the Pokeball</h4>
<div id="pokeball">
<svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
	 viewBox="0 0 204.8 204.8" style="enable-background:new 0 0 204.8 204.8;" xml:space="preserve">
<path style="fill:#EE5281;" d="M201.387,102.4c0-54.67-44.317-98.987-98.987-98.987S3.413,47.73,3.413,102.4H201.387z"/>
<path d="M204.8,105.813H0V102.4C0,45.937,45.937,0,102.4,0s102.4,45.937,102.4,102.4V105.813z M6.886,98.987h191.027
	c-1.802-51.127-43.955-92.16-95.514-92.16S8.689,47.86,6.886,98.987z"/>
<path style="fill:#FFFFFF;" d="M201.387,102.4c0,54.67-44.317,98.987-98.987,98.987S3.413,157.07,3.413,102.4H201.387z"/>
<path d="M102.4,204.8C45.937,204.8,0,158.863,0,102.4c0-1.884,1.529-3.413,3.413-3.413h197.973c1.884,0,3.413,1.529,3.413,3.413
	C204.8,158.863,158.863,204.8,102.4,204.8z M6.886,105.813c1.802,51.127,43.955,92.16,95.514,92.16s93.711-41.033,95.514-92.16
	H6.886z"/>
<circle style="fill:#777777;" cx="102.4" cy="102.4" r="37.547"/>
<path d="M102.4,143.36c-22.586,0-40.96-18.374-40.96-40.96s18.374-40.96,40.96-40.96s40.96,18.374,40.96,40.96
	S124.986,143.36,102.4,143.36z M102.4,68.267c-18.821,0-34.133,15.312-34.133,34.133s15.312,34.133,34.133,34.133
	s34.133-15.312,34.133-34.133S121.221,68.267,102.4,68.267z"/>
<circle style="fill:#FFFFFF;" cx="102.4" cy="102.4" r="23.893"/>
<path d="M102.4,129.707c-15.056,0-27.307-12.25-27.307-27.307s12.25-27.307,27.307-27.307s27.307,12.25,27.307,27.307
	S117.456,129.707,102.4,129.707z M102.4,81.92c-11.293,0-20.48,9.187-20.48,20.48s9.187,20.48,20.48,20.48s20.48-9.187,20.48-20.48
	S113.693,81.92,102.4,81.92z"/>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
<g>
</g>
</svg>
</div>

    </article>
    <div id="scroll-trigger" ref="infinitescrolltrigger">
      <i class="fas fa-spinner fa-spin"></i>
    </div>
  </div>
</template>

<script>
  export default {
    props: [
      'imageUrl',
      'apiUrl'
    ],
    data: () => {
      return {
        pokemons: [],
        nextUrl: '',
        currentUrl: ''
      }
    },
    methods: {
      fetchData() {
        let req = new Request(this.currentUrl);
        fetch(req)
          .then((resp) => {
            if(resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.nextUrl = data.next;
            data.results.forEach(pokemon => {
              pokemon.id = pokemon.url.split('/')
                .filter(function(part) { return !!part }).pop();
              this.pokemons.push(pokemon);
            });
          })
          .catch((error) => {
            console.log(error);
          })
      },
      scrollTrigger() {
        const observer = new IntersectionObserver((entries) => {
          entries.forEach(entry => {
            if(entry.intersectionRatio > 0 && this.nextUrl) {
              this.next();
            }
          });
        });

        observer.observe(this.$refs.infinitescrolltrigger);
      },
      next() {
        this.currentUrl = this.nextUrl;
        this.fetchData();
      },
      setPokemonUrl(url) {
        this.$emit('setPokemonUrl', url);
      }
    },
    created() {
      this.currentUrl = this.apiUrl;
      this.fetchData();
    },
    mounted() {
      this.scrollTrigger();
    }
  }
</script>

<style lang="scss" scoped>
  .list {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    grid-gap: 10px;
    width: 100%;
    max-width: 510px;

    article {
      height: 320px;
      background: linear-gradient(0deg, #ffffff 50%, #b63f3f 50%);
      text-align: center;
      text-transform: capitalize;
      border-radius: 5px;
      cursor: pointer;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);

      h3 {
        margin: 0;
      }
    }
  }

  #scroll-trigger {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 150px;
    font-size: 2rem;
    color: #efefef;
  }


.pokepoke {
	-webkit-animation: pokepoke 2s cubic-bezier(0.550, 0.085, 0.680, 0.530) 1s both;
	        animation: pokepoke 2s cubic-bezier(0.550, 0.085, 0.680, 0.530) 1s both;
}


@-webkit-keyframes pokepoke {
  0% {
    -webkit-filter: blur(12px);
            filter: blur(12px);
    opacity: 0;
  }
  100% {
    -webkit-filter: blur(0px);
            filter: blur(0px);
    opacity: 1;
  }
}
@keyframes pokepoke {
  0% {
    -webkit-filter: blur(12px);
            filter: blur(12px);
    opacity: 0;
  }
  100% {
    -webkit-filter: blur(0px);
            filter: blur(0px);
    opacity: 1;
  }
}

h3,h4 { color: #003A70; font-family: 'Raleway',sans-serif; font-size: 16px; font-weight: 800; line-height: 72px; margin: 0 0 24px; text-align: center; text-transform: uppercase; }

hr {
    border: 4px solid black;
    position: relative;
    top: -46px;
}

h3.pokepoke {
    position: relative;
    top: -20px;
    color: #ffffff;
}

div#pokeball svg {
    width: 50%;
    position: relative;
    top: -50px;
}

#pokeball_text{
font-size: 10px;
}

h4#pokeball_text {
    position: relative;
    top: -43px;
    font-size: 10px;
    line-height: normal;
    color: #000000;
}

#pokeball {
	-webkit-animation: pokeball 0.90s linear infinite both;
	        animation: pokeball 0.90s linear infinite both;
}

@-webkit-keyframes pokeball {
  0% {
    -webkit-transform: translate(0);
    transform: translate(0);
  }
  20% {
    -webkit-transform: translate(-2px, 2px);
    transform: translate(-2px, 2px);
  }
  40% {
    -webkit-transform: translate(-2px, -2px);
    transform: translate(-2px, -2px);
  }
  60% {
    -webkit-transform: translate(2px, 2px);
    transform: translate(2px, 2px);
  }
  80% {
    -webkit-transform: translate(2px, -2px);
    transform: translate(2px, -2px);
  }
  100% {
    -webkit-transform: translate(0);
    transform: translate(0);
  }
  }
@keyframes pokeball {
0% {
-webkit-transform: translate(0);
transform: translate(0);
}
20% {
-webkit-transform: translate(-2px, 2px);
transform: translate(-2px, 2px);
}
40% {
-webkit-transform: translate(-2px, -2px);
transform: translate(-2px, -2px);
}
60% {
-webkit-transform: translate(2px, 2px);
transform: translate(2px, 2px);
}
80% {
-webkit-transform: translate(2px, -2px);
transform: translate(2px, -2px);
}
100% {
-webkit-transform: translate(0);
transform: translate(0);
}
}

img {
    width: 90%;
}

h1 {
      color: #ffffff;
    font-family: 'Raleway',sans-serif;
    font-size: 43px;
    font-weight: 800;
    line-height: 72px;
    margin: 0 0 24px;
    text-align: center;
    text-transform: uppercase;
}

</style>

