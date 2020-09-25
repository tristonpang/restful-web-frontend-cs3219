<template>
  <div id="app">
    <h1>Contacts</h1>
    <div id="form-div">
      <contact-form @add:contact="addContact" />
    </div>
    <contacts-table
      :contacts="contacts"
      @delete:contact="deleteContact"
      @edit:contact="editContact"
    />
  </div>
</template>

<script>
import axios from "axios";

import ContactsTable from "@/components/ContactsTable.vue";
import ContactForm from "@/components/ContactForm.vue";
const apiUrl =
  "https://0uolcs7ym4.execute-api.us-east-1.amazonaws.com/dev/api/contacts";

export default {
  name: "App",
  components: {
    ContactsTable,
    ContactForm,
  },
  data() {
    return {
      contacts: [],
    };
  },
  // Fetches posts when the component is created.
  created() {
    axios
      .get(apiUrl)
      .then((response) => {
        // console.log(response.data);
        this.contacts = response.data.data;
      })
      .catch((e) => {
        console.log(e);
      });
  },
  methods: {
    addContact(contact) {
      let newSavedContact;
      axios
        .post(apiUrl, { ...contact })
        .then((response) => {
          newSavedContact = response.data.data;
          this.contacts = [...this.contacts, newSavedContact];
        })
        .catch((e) => {
          console.log(e);
        });
    },
    deleteContact(contactId) {
      this.contacts = this.contacts.filter(
        (contact) => contact._id !== contactId
      );
      let removedContact = this.contacts.filter(
        (contact) => contact._id === contactId
      )[0];
      axios.delete(apiUrl + "/" + contactId).catch((e) => {
        console.log(e);
        // re-add contact on error
        this.contacts = [...this.contacts, removedContact];
      });
    },
    editContact(contactId, updatedContact) {
      const oldContact = this.contacts.filter(
        (contact) => contact._id === contactId
      )[0];
      
      this.contacts = this.contacts.map((contact) =>
        contact._id === contactId ? updatedContact : contact
      );
      axios.put(apiUrl + "/" + contactId, { ...updatedContact }).catch((e) => {
        console.log(e);
        // undo changes on error
        this.contacts = this.contacts.map((contact) =>
          contact._id === contactId ? oldContact : contact
        );
      });
    },
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
#form-div {
  margin: 2em 25%;
  text-align: left;
}
</style>
