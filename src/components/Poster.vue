<template>
  <article class="poster">
    <div class="header">
      <h1>{{data.headline}}</h1>
      <h1>{{data.headline}}</h1>
    </div>
    <h2>{{data.name}}</h2>
    <img v-if="data.image" :src="data.image" :alt="`Wanted poster for ${data.name}`">
    <section v-if="data.crimes">
      <h3>Wanted for</h3>
      <ul>
        <li v-for="(crime, crimeIndex) in parsedCrimes" :key="crimeIndex">{{crime}}</li>
      </ul>
    </section>
    <div v-for="(custom, customIndex) in data.customFields" :key="customIndex">
      <h3>{{custom.title}}</h3>
      <p>{{custom.text}}</p>
    </div>
    <h2>Reward</h2>
    <p>{{data.reward}}</p>
  </article>
</template>

<script>
export default {
  name: 'Poster',
  props: {
    data: Object
  },
  computed: {
    parsedCrimes () {
      let list = (this.data.crimes.split(','))
      return list.map(Function.prototype.call, String.prototype.trim)
    }
  }
}
</script>

<style lang="scss" scoped>
.poster {
  position: relative;
  outline: solid 1px black;
  height: var(--pageHeight);
  width: var(--pageWidth);
  padding: .5in;
}

h1 {
  font-size: 6em;
  text-align: center;
  font-style: normal;
  font-weight: 400;
  margin: 0;
}

.header {
  position: relative;

  h1:first-child {
    font-family: hwt-american-outline, sans-serif;
  }

  h1:last-child {
    position: absolute;
    top: 0; left: 0;
    width: 100%;
    font-family: hwt-american-inset, sans-serif;
  }
}
</style>
