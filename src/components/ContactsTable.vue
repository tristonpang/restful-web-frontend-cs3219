<template>
  <div id="contacts-table">
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Gender</th>
          <th>Email</th>
          <th>Phone number</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact._id">
          <td v-if="editingId === contact._id">
            <input type="text" v-model="contact.name" />
          </td>
          <td v-else>{{ contact.name }}</td>

          <td v-if="editingId === contact._id">
            <input type="text" v-model="contact.gender" />
          </td>
          <td v-else>{{ contact.gender }}</td>

          <td v-if="editingId === contact._id">
            <input type="text" v-model="contact.email" />
          </td>
          <td v-else>{{ contact.email }}</td>

          <td v-if="editingId === contact._id">
            <input type="text" v-model="contact.phone" />
          </td>
          <td v-else>{{ contact.phone }}</td>
          <td v-if="editingId === contact._id">
            <b-button @click="editContact(contact)" variant="success">Save</b-button>
            <b-button class="muted-button" @click="editingId = null">Cancel</b-button>
          </td>
          <td v-else>
            <b-button @click="editMode(contact._id)" variant="primary">Edit</b-button>
            <b-button variant="danger" @click="handleDelete(contact._id)">
              Delete
            </b-button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "contacts-table",
  data() {
    return {
      editingId: null,
    };
  },
  props: {
    contacts: Array,
  },
  methods: {
    handleDelete(id) {
      this.$emit("delete:contact", id);
    },
    editMode(id) {
      this.editingId = id;
    },
    editContact(contact) {
      if (
        contact.name === "" ||
        contact.email === "" ||
        contact.gender === "" ||
        contact.phone === ""
      )
        return;
      this.$emit("edit:contact", contact._id, contact);
      this.editingId = null;
    },
  },
};
</script>

<style scoped>
div table {
  width: 100%;
  border-collapse: separate;
}
button {
  margin-left: 12px;
}
</style>