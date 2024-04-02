<template>
  <v-data-table :headers="headers" :items="trains" :sort-by="[{ key: 'email', order: 'asc' }]">
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>My CRUD</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <Comp @saveTrain="AddTrain" />
      </v-toolbar>
    </template>

    <template te v-slot:item.name="{ item }">
      <span>{{ item.Name }}</span>
    </template>

    <template v-slot:item.actions="{ item }">
      <v-icon class="me-2" size="small" @click="editItem(item)">mdi-pencil</v-icon>
      <v-icon size="small" @click="deleteItem(item)">mdi-delete</v-icon>
      <v-icon size="small" @click="detailsItem(item)">mdi-details</v-icon>
      
    </template>

    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>

  <v-dialog v-model="dialogDelete" max-width="500px">
    <v-card>
      <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
        <v-btn color="blue darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
        <v-spacer></v-spacer>
      </v-card-actions>
    </v-card>
  </v-dialog>
  <Détails :train="editedItem" :showDialog="dialogDetails" @update:showDialog="updateDialogDetails" />
  <Edit :train="editedItem" :teachers="teachers" :students="students" @save-changes="EditTrains" ref="editItemDialog" />

</template>
  
  <script>
import Comp from "./dialogs/Add.vue";
import Détails from "./dialogs/Détails.vue";
import Edit from "./dialogs/Edit.vue";
  
  export default {
    components: {
      Comp,
      Détails,
      Edit
    },
     data() {
      return {
        trains: [],
        teachers: [
        { firstName: "Mr", lastName: "Saci" },
        { firstName: "Mr", lastName: "Ramdani" },
        { firstName: "Mrs", lastName: "Malak" }
      ],
      students: [
        { firstName: "Kamel", lastName: "Mrabet" },
        { firstName: "Cristiano", lastName: "Ronaldo" },
        { firstName: "Myke", lastName: "Tyson" }
      ],
        dialog: false,
        dialogDelete: false,
        editedItem: {},
        dialogDetails: false,
        headers: [
          {
            title: "Name",
            align: "start",
            sortable: false,
            value: "name"
          },
          { title: "teacher", value: "teacher" },
          { title: "Students", value: "student" },
          { title: "Actions", value: "actions", sortable: false }
        ],

      };
    },
    created() {
      this.initialize();
    },
    methods: {
      initialize() {
        this.trains = [
          {
            Name: "Kamel",
            teacher: "Mr Saci",
            student: "Kamel Mrabet",
          },
          {
              Name: "Cristiano",
            teacher: "Mr Ramdani",
            student: "Cristiano Ronaldo",
          },
          {
              Name: "Mike",
            teacher: "Mrs Malki",
            student: "myke tyson",
          }
        ];
      },
      editItem(item) {
        this.editedIndex = this.trains.indexOf(item);
        this.editedItem = Object.assign({}, item);
        this.$nextTick(() => {
          this.$refs.editItemDialog.openDialog(); 
        }); 
      },
      deleteItem(item) {
        this.editedIndex = this.trains.indexOf(item);
        this.editedItem = Object.assign({}, item);
        this.dialogDelete = true;
      },
      closeDelete() {
        this.dialogDelete = false;
      },
      deleteItemConfirm() {
        this.trains.splice(this.editedIndex, 1);
        this.closeDelete();
      },
      detailsItem(item) {
        this.editedItem = Object.assign({}, item); 
        this.dialogDetails = true; 
      },
      closeDetails() {
        this.dialogDetails = false;
      },
      updateDialogDetails(value) {
        this.dialogDetails = value;
      },
      EditTrains(updatedTrains) {
    this.trains.splice(this.editedIndex, 1, updatedTrains);
},
      AddTrain(TrainData) {
        if (TrainData) {
          this.trains.push(TrainData);
        } else {
          console.error("Error: train data not received from NewItemCom.vue");
        }
      }
    }
  };
  </script>
  