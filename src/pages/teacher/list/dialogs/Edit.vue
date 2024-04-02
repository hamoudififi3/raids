<template>
  <v-dialog v-model="dialog" max-width="500px">
    <v-card>
      <v-card-title class="text-h5">Edit Teacher</v-card-title>
      <v-card-text>
        <v-text-field v-model="editedTeacher.firstName" label="First Name"></v-text-field>
        <v-text-field v-model="editedTeacher.lastName" label="Last Name"></v-text-field>
        <v-text-field v-model="editedTeacher.email" label="Email"></v-text-field>
        <v-text-field v-model="editedTeacher.phone" label="Phone"></v-text-field>
        <v-text-field v-model="editedTeacher.status" label="Status"></v-text-field>
      </v-card-text>
      <v-card-actions>
        <v-btn color="blue darken-1" @click="saveChanges">Save</v-btn>
        <v-btn color="blue darken-1" @click="closeDialog">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: {
    teacher: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      dialog: false,
      editedTeacher: {}
    };
  },
  methods: {
    openDialog() {
      this.dialog = true;
      this.editedTeacher = { ...this.teacher };
    },
    saveChanges() {
      this.$emit('save-changes', this.editedTeacher);
      this.dialog = false;
    },
    closeDialog() {
      this.dialog = false;
    }
  }
};
</script>
