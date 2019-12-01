<template>
  <main id="app">
    <h1 class="title">Wanted Poster Generator</h1>
      <form>
      <div class="pageForm">
        <fieldset>
          <legend>Page Size</legend>
          <label>
            <input type="radio" name="pageSize" id="letter" value="1" v-model="pageSize">
            8.5"x11"
          </label>
          <label>
            <input type="radio" name="pageSize" id="a4" value="2" v-model="pageSize">
            A4
          </label>
        </fieldset>

        <fieldset>
          <legend>Page Layout</legend>
          <label>
            <input type="radio" name="pageLayout" id="fullPage" v-model="pageLayout" value="1">
            Full page
          </label>
          <label>
            <input type="radio" name="pageLayout" id="halfPage" v-model="pageLayout" value="2">
            Half page, 1 poster
          </label>
          <label>
            <input type="radio" name="pageLayout" id="halfPageTwoUp" v-model="pageLayout" value="3">
            Half page, 2 posters
          </label>
        </fieldset>

        <fieldset>
          <legend>Background</legend>
          <label>
            <input type="radio" name="background" id="bgColor" v-model="background" value="1">
            Color
          </label>
          <label>
            <input type="radio" name="background" id="bgGrey" v-model="background" value="2">
            Greyscale
          </label>
          <label>
            <input type="radio" name="background" id="bgBw" v-model="background" value="3">
            Black and white
          </label>
          <label>
            <input type="radio" name="background" id="bgNone" v-model="background" value="4">
            None
          </label>

        </fieldset>

        <fieldset>
          <legend>Poster Layout</legend>
          <label>
            <input type="radio" name="layout" id="layoutLarge" v-model="layout" value="1">
            Large photo
          </label>
          <label>
            <input type="radio" name="layout" id="layoutSmall" v-model="layout" value="2">
            Small photo
          </label>
          <label>
            <input type="radio" name="layout" id="layoutNone" v-model="layout" value="3">
            No photo
          </label>
        </fieldset>
      </div>

      <data-form class="secondPosterForm" v-model="poster2.data" v-if="pageLayout == 3" />
      <data-form class="firstPosterForm" v-model="poster1.data" />

    </form>

    <section
      class="page"
      :class="{ 'page-twoUp': pageLayout == 2 || pageLayout == 3 }"
      :style="pageSize == 1 ? '--pageWidth: 8.5in; --pageHeight: 11in;' : '--pageWidth: 210mm; --pageHeight: 297mm;'">
      <div class="page_half">
        <poster
          :data="poster1.data"
        />
      </div>
      <div class="page_half">
        <poster
          v-if="pageLayout == 3"
          :data="poster2.data"
        />
      </div>
    </section>
  </main>
</template>

<script>
import Poster from './components/Poster.vue'
import DataForm from './components/DataForm.vue'

export default {
  name: 'app',
  components: {
    Poster,
    DataForm
  },
  data () {
    return {
      pageLayout: 1,
      pageSize: 1, // 1 - 8.5 x 11, 2 - A4
      background: 1,
      layout: 1,
      headlineFont: 1,
      headingFont: 1,
      bodyFont: 1,
      poster1: {
        data: {}
      },
      poster2: {
        data: {}
      }
    }
  }
}
</script>

<style>
*, *::before, *::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  display: grid;
  grid-template-areas:
    "header header"
    "form preview";
  grid-template-columns: max-content 1fr;
  grid-template-rows: auto min-content;
  align-items: start;
}
</style>

<style lang="scss" scoped>
#app {
  display: contents;
}

.title {
  grid-area: header;
}

form {
  grid-area: form;
  display: grid;
  grid-template-rows: repeat(2, auto);
  grid-template-columns: repeat(2, min-content);
}

.pageForm {
  grid-column: span 2;
}

.firstPosterForm {
  grid-column: 1 / 3;
  grid-row: 2;
}

.secondPosterForm {
  grid-column: 2;
  grid-row: 2;

  &+ .firstPosterForm {
    grid-column: 1;
  }
}

.page {
  grid-area: preview;
  outline: solid 1px black;
  width: var(--pageWidth);
  height: var(--pageHeight);
}

.page-twoUp {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  width: var(--pageHeight);
  height: var(--pageWidth);
}

.page-twoUp ::v-deep .poster {
    height: var(--pageWidth);
    width: var(--pageHeight / 2);
  }
</style>
