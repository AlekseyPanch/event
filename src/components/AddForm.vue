<template>
  <form @submit.prevent="onSubmit">
    <md-field>
      <label>Title</label>
      <md-input v-model="title"> </md-input>
    </md-field>
    <md-field>
      <label>Description</label>
      <md-input v-model="description"> </md-input>
    </md-field>
    <md-field>
      <label>Estimate</label>
      <md-input type="number" v-model="estimate"> </md-input>
    </md-field>
    <md-field class="select-input">
      <md-select v-model="priority" name="movie" id="movie">
        <md-option v-for="option in priorities" v-bind:value="option.value" :key="option.value">{{option.text}}</md-option>
      </md-select>
    </md-field>

    <md-button type="submit">Submit</md-button>
  </form>
</template>

<script>
import moment from 'moment'

export default {
  data () {
    return {
      title: '',
      description: '',
      priority: 0,
      estimate: 0,
      priorities: [
        { text: 'Low', value: 0 },
        { text: 'Middle', value: 10 },
        { text: 'High', value: 20 }
      ]
    }
  },

  methods: {
    onSubmit () {
      if (!this.title.length || !this.description.length) {
        return
      }

      const newEvent = {
        title: this.title,
        description: this.description,
        estimate: this.estimate,
        priority: this.priority,
        state: 0,
        create_date: moment().format('YYYY-MM-DD hh:mm:ss')
      }

      this.$emit('add-event', newEvent)

      this.title = ''
      this.description = ''
      this.estimate = ''
      this.priority = ''
    }
  }
}
</script>

<style lang="scss">
  .md-dialog-container {
    padding: 1rem;
    form {
      display: flex;
      justify-content: center;
      flex-direction: column;
    }
  }
  .md-select-menu {
    z-index: 9999;
  }
</style>
