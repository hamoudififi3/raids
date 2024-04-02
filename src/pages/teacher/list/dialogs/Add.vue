<template>
  <v-dialog v-model="dialog" max-width="500px">
    <template v-slot:activator="{ props }">
      <v-btn class="mb-2" color="primary" dark v-bind="props">New Item</v-btn>
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5">Add Teacher</span>
      </v-card-title>

      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12" md="4" sm="6">
              <v-text-field
                v-model="editedItem.firstName"
                label="First Name"
                :rules="[name => !!name || 'First Name is required']"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4" sm="6">
              <v-text-field
                v-model="editedItem.lastName"
                label="Last Name"
                :rules="[name => !!name || 'Last Name is required']"
              >
                <template v-slot:error="{ input }">
                  <div class="error--text" v-if="!input.length">Name cannot be empty.</div>
                </template>
              </v-text-field>
            </v-col>
            <v-col cols="12" md="4" sm="6">
              <v-text-field
                v-model="editedItem.email"
                label="Email"
                :rules="[email => !!email || 'Email is required',
                  email => /^\S+@\S+\.\S+$/.test(email) || 'Invalid email format'
                ]"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4" sm="6">
              <v-text-field 
                v-model="editedItem.phone" 
                label="Phone" 
                :rules="[phone => (phone && /^(06|05|07)\d{8}$/.test(phone)) || 'Invalid phone number: Must be non-empty and contain only digits And More Then 9 digits']"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4" sm="6">
              <v-text-field v-model="editedItem.status" :rules="[status => {
              const validStatuses = ['Married', 'Single', 'Divorced'];
              return validStatuses.includes(status) || 'Status must be Married, Single, or Divorced';
            }]" label="Status"></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" variant="text" @click="close">Cancel</v-btn>
        <v-btn color="blue darken-1" variant="text" @click="save">Save</v-btn>
        <v-spacer></v-spacer>
      </v-card-actions>
    </v-card>
  </v-dialog>
  
</template>

<script>

export default {
  data() {
    return {
      dialog: false,
      editedItem: {
          firstName: "",
          lastName: "",
        email: "",
        phone: 0,
        status: ""
      },
      defaultItem: {
          firstName: "",
          lastName: "",
        email: "",
        phone: 0,
        status: ""
      }
    };
  },
  methods: {
    close() {
      this.dialog = false;
      this.resetForm();
    },
    save() {
      if (
        this.editedItem.firstName &&
        this.editedItem.lastName &&
        this.editedItem.email &&
        this.editedItem.phone && 
        this.editedItem.status
      ) { 
          this.$emit("saveTeacher", this.editedItem);
          this.close();
        } else {
        console.error("Error: Please fill out all required fields.");
      }
    },
    resetForm() {
      this.editedItem = Object.assign({}, this.defaultItem);
    }
  }
};
</script>
