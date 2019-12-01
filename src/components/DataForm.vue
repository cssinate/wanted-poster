<template>
  <div>
    <fieldset class="posterContent">
      <legend>Poster content</legend>
      <label>
        Headline
        <input type="text" v-model="headline">
      </label>
      <label>
        Name
        <input type="text" v-model="name">
      </label>
      <label>
        Crimes<br>
        <small>(list, separated by commas)</small>
        <textarea v-model="crimes"></textarea>
      </label>
      <label>
        Reward
        <input type="text" v-model="reward">
      </label>
      <label>
        Image
        <input type="file" @change="addImage()" ref="fileInput">
      </label>
    </fieldset>

    <fieldset>
      <legend>Custom Fields</legend>
      <div v-for="(custom, index) in customFields" :key="index">
        {{custom.title}}: {{custom.text}}
        <button @click.prevent="removeCustom(index)">Remove</button>
      </div>
      <div>
        <label>
          Section title
          <input type="text" ref="newCustomTitle">
        </label>
        <label>
          Section text
          <input type="text" ref="newCustomText">
        </label>
        <button @click="newCustom($event)" type="button">Add</button>
      </div>
    </fieldset>
  </div>
</template>

<script>
export default {
  props: {
    input: Object
  },
  data () {
    return {
      headline: 'Wanted',
      name: 'Butch Cassidy',
      image: '',
      reward: '800 Gold Pieces',
      crimes: 'Robbery, Aggravated Assault, Crimes Against the King',
      customFields: []
    }
  },
  methods: {
    removeCustom (index) {
      this.customFields.splice(index, 1)
    },
    newCustom (e) {
      e.stopPropagation()
      var title = this.$refs.newCustomTitle
      var text = this.$refs.newCustomText
      if (!title.value && !title.text) {
        return
      }
      this.customFields.push({ title: title.value, text: text.value })
      title.value = ''
      text.value = ''
    },
    addImage () {
      const file = this.$refs.fileInput.files[0]
      const reader = new FileReader()
      /* eslint-disable-next-line */
      let self = this;

      reader.addEventListener('load', function () {
        self.image = reader.result
      }, false)

      if (file) {
        reader.readAsDataURL(file)
      }
    }
  },
  mounted () {
    this.$emit('input', this.$data)
  },
  watch: {
    $data () {
      this.$emit('input', this.$data)
    }
  }
}
</script>

<style lang="scss">
.posterContent, .posterContent label {
  display: flex;
  flex-direction: column;
}

.posterContent label:not(:last-child) {
  margin-bottom: .75rem;
}
</style>
