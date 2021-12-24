<template>
  <div>
    <!-- Note -->
    <div class="note" v-if="!editMode">
      <header>
        <h3>{{ note.title }}</h3>

        <div class="spacer"></div>

        <i v-if="!settingsOpen" @click="toggleSettings()" class="fas fa-bars"></i>
        <i v-if="settingsOpen" @click="onDelete()" class="fas fa-trash"></i>
        <i v-if="settingsOpen" @click="toggleMode()" class="fas fa-pen"></i>
      </header>
      
      <p class="note-text">{{ note.text }}</p>
    </div>

    <!-- Note Editor -->
    <div class="outline-note" v-else>
      <form @submit.prevent="submitEdit()">
        <input type="text" class="input" placeholder="Title" v-model="title">
        <textarea placeholder="Text" v-model="text"></textarea>

        <p class="error">{{ error }}</p>

        <button type="submit" class="btn blue">Edit</button>
        <button type="button" class="btn red" @click="cancelEdit()">Cancel</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    note: Object
  },
  data() {
    return {
      editMode: false,
      settingsOpen: false,
      title: this.note.title,
      text: this.note.text,
      error: ''
    }
  },
  methods: {
    toggleMode() {
      this.editMode = this.editMode ? false : true;
    },
    toggleSettings() {
      this.settingsOpen = this.settingsOpen ? false : true;
    },
    async submitEdit() {
      this.note.title = this.title;
      this.note.text = this.text;
      this.toggleMode();
      this.toggleSettings();

      const config = {
        headers: {
          Accept: 'application/json',
          Authorization: `Bearer ${localStorage.getItem('jwt')}`
        }
      }

      await axios.put(`http://localhost:1337/api/notes/${this.note.id}`, {
        data: {
          title: this.title,
          text: this.text
        }
      }, config);
    },
    cancelEdit() {
      this.title = this.note.title;
      this.text = this.note.text;
      this.toggleMode();
      this.toggleSettings();
    },
    onDelete() {
      this.$emit('delete-note', this.note);
    }
  }
}
</script>

<style scoped>
  .note {
    padding: 0.5em;
    border: solid black 2px;
    border-radius: 10px;
    margin: 1em;
    width: 250px;
  }

  h3 {
    margin: 0;
  }

  .note-text {
    white-space: pre-line;
    margin-top: 0.5em;
  }

  header {
    display: flex;
    align-items: center;
    margin-top: 0.5em;
  }

  .fas {
    margin: 0;
    margin-left: 1em;
    cursor: pointer;
  }

  .fa-trash {
    color: rgb(223, 16, 16);
  }
  .fa-pen {
    color: rgb(4, 195, 253);
  }
</style>