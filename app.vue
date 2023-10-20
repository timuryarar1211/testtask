<template>
  <div class="notes-app">
    <div class="sidebar">
      <div class="note-list">
        <div class="search">
          <input v-model="searchQuery" placeholder="Search Notes" />
        </div>
        <div v-for="note in filteredNotes" :key="note.id" class="note-item">
          <div @click="selectNote(note)" :class="{ active: selectedNote === note }">
            Title: {{ note.title }}
          </div>
          <div @click="selectNote(note)" :class="{ active: selectedNote === note }">
            Description: {{ note.description }}
          </div>
          <div @click="selectNote(note)" :class="{ active: selectedNote === note }">
            Content: {{ note.content }}
          </div>
          <button @click="deleteNote(note)" class="delete-button">Delete</button>
        </div>
      </div>
      <div class="add-note">
        <input v-model="newNoteTitle" placeholder="New Note Title" />
        <button @click="createNote">Add Note</button>
      </div>
    </div>
    <div class="note-editor">
      <div class="note-details" v-if="selectedNote">
        <input v-model="selectedNote.title" :readonly="!selectedNote.editing" />
        <textarea v-model="selectedNote.content" :readonly="!selectedNote.editing" placeholder="Note content"></textarea>
        <input v-model="selectedNote.description" :readonly="!selectedNote.editing" placeholder="Note description" />
        <button @click="editSelectedNote" class="edit-button" v-if="!selectedNote.editing">Edit</button>
        <button @click="saveSelectedNote" class="edit-button" v-if="selectedNote.editing">Save</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notes: [
        { id: 1, title: 'Note 1', content: 'This is the content of Note 1', description: 'Description for Note 1', editing: false },
        { id: 2, title: 'Note 2', content: 'This is the content of Note 2', description: 'Description for Note 2', editing: false },
      ],
      newNoteTitle: '',
      selectedNote: null,
      searchQuery: '',
    };
  },
  computed: {
    filteredNotes() {
      return this.notes.filter((note) => {
        const titleMatch = note.title.toLowerCase().includes(this.searchQuery.toLowerCase());
        return titleMatch;
      });
    },
  },
  methods: {
    createNote() {
      const newNote = {
        id: Date.now(),
        title: this.newNoteTitle,
        content: '',
        description: '',
        editing: false,
      };
      this.notes.push(newNote);
      this.saveNotesToIndexedDB();
      this.newNoteTitle = '';
    },
    selectNote(note) {
      this.selectedNote = note;
    },
    editSelectedNote() {
      if (this.selectedNote) {
        this.selectedNote.editing = true;
      }
    },
    saveSelectedNote() {
      if (this.selectedNote) {
        this.selectedNote.editing = false;
      }
      this.saveNotesToIndexedDB();
    },
    deleteNote(note) {
      const index = this.notes.indexOf(note);
      if (index !== -1) {
        this.notes.splice(index, 1);
        this.saveNotesToIndexedDB();
        if (this.selectedNote === note) {
          this.selectedNote = null;
        }
      }
    },
    updateNoteDescription(note) {
      this.saveNotesToIndexedDB();
    },
    loadNotesFromIndexedDB() {
      // Load notes from IndexedDB and initialize the 'notes' data
    },
    saveNotesToIndexedDB() {
      // Save notes to IndexedDB
    },
  },
};
</script>

<style scoped>
.notes-app {
  display: flex;
  font-family: Arial, sans-serif;
  width: 100%;
  height: 100vh;
  background: #f5f5f5;
}

.sidebar {
  flex: 1;
  padding: 20px;
  background: #f0f0f0;
  overflow-y: auto;
}

.note-list {
  border-right: 1px solid #ddd;
  padding-right: 20px;
}

.note-item {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px 0;
  cursor: pointer;
}

.note-item.active {
  background: #007BFF;
  color: white;
  font-weight: bold;
}

.note-description {
  font-style: italic;
}

.note-content {
  white-space: pre-wrap;
}

.delete-button {
  background: #FF3B30;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}

.add-note {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
  align-items: center;
}

.add-note input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-right: 10px;
}

.add-note button {
  background: #007BFF;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.note-editor {
  flex: 2;
  padding: 20px;
}

.note-details input, .note-details textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin: 10px 0;
}

.note-details textarea {
  height: 200px;
}

.search {
  margin-bottom: 10px;
}

.edit-button {
  background: #007BFF;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}
</style>
