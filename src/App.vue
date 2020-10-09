<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-4 container">
          <ContactForm v-on:created="getAllContacts" />
        </div>
        <div class="col-8">
          <ContactList
            v-bind:contacts="contacts"
            v-on:delete-contact="deleteContact"
            v-on:save-edit="saveEdit"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ContactList from '@/components/ContactList.vue';
import ContactForm from './components/ContactForm.vue';
import db from './shared/db';

export default {
  data() {
    return {
      contacts: [],
    };
  },
  created() {
    this.getAllContacts();
  },
  methods: {
    getAllContacts() {
      db.read().then((snapshot) => {
        this.contacts = snapshot.docs;
      }).catch((error) => {
        console.error(error);
      });
    },
    deleteContact(contact) {
      db.delete(contact.id).then(() => {
        this.getAllContacts();
      }).catch((error) => {
        console.error(error);
      });
    },
    saveEdit(editedContact) {
      db.update(editedContact).then(() => {
        this.getAllContacts();
      }).catch((error) => {
        console.error(error);
      });
    },
  },
  name: 'app',
  components: {
    ContactForm,
    ContactList,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
