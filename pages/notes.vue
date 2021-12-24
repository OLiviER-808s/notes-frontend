<template>
  <div class="notes">
    <Toolbar/>

    <div class="grid" ref="grid">
      <div v-for="row in layout" :key="layout.indexOf(row)">
        <div v-for="note in row" :key="note.id">
          <AddNote v-if="note.addNote" @new-note="addNote"/>
          <Note v-else :note="note" @delete-note="deleteNote"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Note from '~/components/Note.vue'
import axios from 'axios';
import AddNote from '~/components/AddNote.vue';
import Toolbar from '~/components/Toolbar.vue';

export default {
  components: { Note, AddNote, Toolbar },
  head: {
    title: 'My Notes',
    link: [
      { rel: 'stylesheet', href: "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css" }
    ]
  },
  data() {
    return {
      notes: [],
      rows: 0,
      layout: [],
      user: { }
    }
  },
  async created() {
    const user = JSON.parse(localStorage.getItem('user'));
    this.user = user;

    const config = {
      headers: {
        Accept: 'application/json',
        Authorization: `Bearer ${localStorage.getItem('jwt')}`
      }
    }

    const res = await axios.get(`http://localhost:1337/api/notes?filters[authorEmail][$eq]=${user.email}&sort=publishedAt:desc`, config);
    this.notes = res.data.data.map(note => {
      return { ...note.attributes, id: note.id }
    });
    
    this.rearrangeNotes();
    window.addEventListener('resize', this.rearrangeNotes);
  },
  methods: {
    rearrangeNotes() {
      const rows = Math.floor(this.$refs.grid.clientWidth / 270);

      this.rows = rows;
      this.layout = []

      for (let i = 0; i < rows; i++) {
        this.layout.push([]);
      }

      if (this.notes.length === 0 || !this.notes[0].addNote) this.notes.unshift({ id: 'addNote', addNote: true });
      let c = 0;
      for (let i = 0; i < this.notes.length; i++) {
        this.layout[c].push(this.notes[i]);
        
        if (c < rows - 1) c++;
        else c = 0;
      }
    },
    async addNote(note) {
      this.notes.shift();
      this.notes.unshift({ ...note, id: this.notes.length });
      this.rearrangeNotes();

      const user = JSON.parse(localStorage.getItem('user'));
      const config = {
        headers: {
          Accept: 'application/json',
          Authorization: `Bearer ${localStorage.getItem('jwt')}`
        }
      }

      await axios.post('http://localhost:1337/api/notes', {
        data: {
          ...note,
          authorEmail: user.email
        }
      }, config);
    },
    async deleteNote(note) {
      this.notes = this.notes.filter(n => n.id !== note.id);
      this.rearrangeNotes();

      const config = {
        headers: {
          Accept: 'application/json',
          Authorization: `Bearer ${localStorage.getItem('jwt')}`
        }
      }

      await axios.delete(`http://localhost:1337/api/notes/${note.id}`, config);
    }
  }
}
</script>

<style>
  .grid {
    display: flex;
    justify-content: center;
    width: 100%;
  }

  .notes {
    width: 100%;
  }

  .logout-bar {
    text-align: center;
    width: 100%;
  }
</style>