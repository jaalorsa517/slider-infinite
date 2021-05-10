<template lang="pug">
  .carousel
    .container
      .container-img(v-for="pokemon,index of pokemons" :key="index")
        img.img(:src="pokemon.img" :alt="pokemon.name" )
</template>

<script>
  export default {
    name: 'HelloWorld',
    data: function() {
      return {
        pokemons: [],
      };
    },
    created: function() {
      const uri = 'https://pokeapi.co/api/v2/pokemon?limit=10';
      fetch(uri)
        .then(response => response.json())
        .then(json => {
          Promise.all(json.results.map(pokemon => fetch(pokemon.url))).then(resp =>
            Promise.all(resp.map(r => r.json())).then(pokemons => {
              pokemons.forEach(pokemon => {
                this.pokemons.push({
                  name: pokemon.name,
                  img: pokemon.sprites.other['official-artwork']['front_default'],
                });
              });
              this.pokemons.forEach(pokemon => this.pokemons.push(pokemon));
            })
          );
        });
    },
  };
</script>

<style lang="stylus">
  @keyframes movie {
     0%{transform:translateX(0)}
     100%{transform:translateX(calc(-240px * 10))}
   }
  .carousel
    width 100%
    height 240px
    position relative
    margin auto
    overflow hidden

  .container
    animation movie 10s linear infinite
    width calc(240px * 20)
    display flex

    .container-img
      width 240px
      height 240px

      .img
        height 240px
        width 240px
</style>
