<template>
  <div class="outline-note">
    <div class="inactive" v-if="!active">
      <button @click="toggleMode()" class="btn green">Add Note</button>
    </div>

    <div class="active" v-if="active">
      <form @submit.prevent="onSubmit()">
        <input type="text" class="input" placeholder="Title" v-model="title">
        <textarea placeholder="Text" v-model="text"></textarea>

        <p class="error">{{ error }}</p>

        <button type="submit" class="btn green">Create</button>
        <button @click="toggleMode()" type="button" class="btn red">Cancel</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      active: false,
      title: '',
      text: '',
      error: ''
    }
  },
  methods: {
    toggleMode() {
      this.active = this.active ? false : true;
    },
    onSubmit() {
      this.error = '';

      if (!this.title && !this.text) {
        this.error = 'Note must include a title or text';
      }
      else {
        this.$emit('new-note', {
          title: this.title,
          text: this.text
        });

        this.title = '';
        this.text = '';
        this.toggleMode();
      }
    }
  }
}
</script>

<style>
  .outline-note {
    padding: 0.5em;
    border: dashed black 2px;
    border-radius: 10px;
    margin: 1em;
    width: 250px;
  }
  
  .inactive {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 9em;
    width: 100%;
  }

  .active {
    padding: 0.5em;
  }

  textarea {
    border-radius: 7px;
    background-color: rgb(221, 219, 219);
    color: black;
    border: none;
    display: block;
    margin-bottom: 1em;
    padding: 0.5em;
    width: 95%;
    height: 9em;
    resize: vertical;
  }
</style>