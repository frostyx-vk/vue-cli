<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">

          <message v-if="message" :message="message" />
          <newNote :note="note" @addNote="addNote" />
          <div class="note-header" style="margin: 20px 0;">
            <h1>{{ title }}</h1>
            <search :value="search" placeholder="Найти заметку" @search="search = $event" />
            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24"
                height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                stroke-linejoin="round">
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
              </svg>
              <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24"
                height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                stroke-linejoin="round">
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3" y2="6"></line>
                <line x1="3" y1="12" x2="3" y2="12"></line>
                <line x1="3" y1="18" x2="3" y2="18"></line>
              </svg>
            </div>
          </div>

          <notes :notes="notesFilter" @remove="removeNote" :grid="grid" />

        </div>
      </section>
    </div>
  </div>
</template>

<script>
import message from '@/components/Message.vue'
import newNote from '@/components/NewNote.vue'
import notes from '@/components/Notes.vue'
import search from '@/components/Search.vue'
export default {
  components: {
    message,
    newNote,
    notes,
    search,
  },
  data() {
    return {
      title: 'Заметки',
      search: '',
      message: null,
      grid: true,
      note: {
        title: '',
        descr: '',
      },
      notes: [
        {
          title: 'First',
          descr: 'For First',
          date: new Date(Date.now()).toLocaleString(),
        },
        {
          title: 'Second',
          descr: 'For second',
          date: new Date(Date.now()).toLocaleString(),
        },
        {
          title: 'Third',
          descr: 'For third',
          date: new Date(Date.now()).toLocaleString(),
        },
      ],
    }
  },
  computed: {
    notesFilter() {
      let array = this.notes;
      let search = this.search;

      if (!search) return array;

      search = search.trim().toLowerCase();

      array = array.filter(item => {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item
        };
      });

      return array
    }
  },
  methods: {
    addNote() {
      let { title, descr } = this.note;

      if (title === '') {
        this.message = 'Заголовок не заполнен!';
        return false;
      };

      this.notes.push({
        title: title,
        descr: descr,
        date: new Date(Date.now()).toLocaleString(),
      });

      this.massage = null;
      this.note.title = '';
      this.note.descr = '';
    },
    removeNote(index) {
      this.notes.splice(index, 1)
    },
  }
}
</script>

<style>
.container h1 {
  text-align: center;
  padding-bottom: 30px;
}

@media (max-width: 768px) {
  .note-header {
    flex-direction: column;
    gap: 14px;
  }
  .container h1 {
    padding-bottom: 0;
  }
}
</style>
