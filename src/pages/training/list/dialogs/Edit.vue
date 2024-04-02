<template>
  <v-dialog v-model="dialog" max-width="500px">
    <v-card>
      <v-card-title class="text-h5">Edit Train</v-card-title>
      <v-card-text>
        <v-text-field v-model="editedTrain.Name" label="Name"></v-text-field>
        <v-select v-model="editedTrain.teacher" :items="formattedTeachers" item-text="fullName" label="Select Teacher"></v-select>
        <v-select v-model="editedTrain.student" :items="formattedStudents" item-text="fullName" label="Select Student"></v-select>
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
    train: {
      type: Object,
      required: true
    },
    teachers: {
      type: Array,
      required: true
    },
    students: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      dialog: false,
      editedTrain: {}
    };
  },
  computed: {
    formattedTeachers() {
      return this.teachers.map(teacher => `${teacher.firstName} ${teacher.lastName}`);
    },
    formattedStudents() {
      return this.students.map(student => `${student.firstName} ${student.lastName}`);
    }
  },
  methods: {
    openDialog() {
      this.dialog = true;
      this.editedTrain = { ...this.train };
    },
    saveChanges() {
      this.$emit('save-changes', this.editedTrain);
      this.dialog = false;
    },
    closeDialog() {
      this.dialog = false;
    }
  }
};
</script>
