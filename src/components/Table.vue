<template>
  <div class="table">
    <div class="form">
      <md-dialog :md-active.sync="showForm">
        <add-form @add-event="addEvent" :showForm="showForm" />
        <div class="close-button">
          <span @click="showForm = false" class="close"></span>
        </div>
      </md-dialog>
      <md-button class="add-btn" @click="showForm = true">Добавить</md-button>
    </div>
    <md-table>
      <md-table-row>
        <md-table-head>Title</md-table-head>
        <md-table-head>Description</md-table-head>
        <md-table-head>State</md-table-head>
        <md-table-head>Estimate</md-table-head>
        <md-table-head>Priority</md-table-head>
        <md-table-head>Date</md-table-head>
      </md-table-row>

      <md-table-row v-for="event in tableData" :key="event.create_date">
        <md-table-cell md-numeric>{{ event.title }}</md-table-cell>
        <md-table-cell class="description-content">{{ event.description }}</md-table-cell>
        <md-table-cell>{{ event.state }}</md-table-cell>
        <md-table-cell>{{ event.estimate }}</md-table-cell>
        <md-table-cell>{{ event.priority }}</md-table-cell>
        <md-table-cell>{{ event.create_date }}</md-table-cell>
        <md-table-cell>
          <md-button class="close-ripple-button" @click="removeEvent(event.create_date)">x</md-button>
        </md-table-cell>
      </md-table-row>
    </md-table>
    <md-dialog-confirm
      :md-active.sync="showAlert"
      md-title="Вы уверены?"
      md-confirm-text="Да"
      md-cancel-text="Нет"
      @md-cancel="showAlert = false"
      @md-confirm="confirmAlert"> </md-dialog-confirm>
  </div>
</template>

<script>
import data from '../data/data.json'
import AddForm from './AddForm.vue'

export default {
  components: {
    'add-form': AddForm
  },

  methods: {
    async addEvent (event) {
      try {
        // mock server request
        await Promise.resolve()
        this.tableData.unshift(event)
        this.showForm = false
      } catch (err) {
        console.log(err)
      }
    },

    removeEvent (eventDate) {
      this.idRemoveItem = eventDate
      this.showAlert = true
    },

    async confirmAlert () {
      // mock server request
      await Promise.resolve()

      const removeIndex = this.tableData.findIndex(t => t.create_date === this.idRemoveItem)
      this.tableData.splice(removeIndex, 1)

      this.idRemoveItem = null
    }
  },

  data () {
    return {
      tableData: data,
      showForm: false,
      showAlert: false,
      idRemoveItem: null
    }
  }
}
</script>

<style lang="scss">
  .form {
    display: flex;
    justify-content: center;

    .add-btn {
      background-color: #c3c3c3;
    }
  }
  @media screen and (max-width: 768px) {
    .form {
      height: 100%;
      justify-content: space-around;
    }
  }

  .table {
    .close-ripple-button {
      padding: 0;
    }
  }

  .close-ripple-button {
    width: 3rem;
    height: 3rem;
  }
  .description-content {
    min-width: 300px;
  }
  .close {
    position: absolute;
    right: 5px;
    top: 5px;
    cursor: pointer;
    width: 32px;
    height: 32px;
    opacity: 0.3;
  }
  .close:hover {
    opacity: 1;
  }
  .close:before, .close:after {
    position: absolute;
    left: 15px;
    content: ' ';
    height: 33px;
    width: 2px;
    background-color: #333;
  }
  .close:before {
    transform: rotate(45deg);
  }
  .close:after {
    transform: rotate(-45deg);
  }
</style>
