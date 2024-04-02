  <template>
      <v-dialog v-model="dialog" max-width="500px">
        <template v-slot:activator="{ props }">
          <v-btn class="mb-2" color="primary" dark v-bind="props">Add List</v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="text-h5">Add Item</span>
          </v-card-title>
    
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" md="4" sm="6">
                  <v-text-field
                    v-model="editedItem.Name"
                    label="Name"
                    :rules="[name => !!name || 'Name is required']"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" md="4" sm="6">
                  <v-select v-model="this.editedItem.teacher" :items="formattedTeachers" item-text="firstName" item-value="lastName" label="Select Teacher"></v-select>
                </v-col>
                <v-col cols="12" md="4" sm="6">
                  <v-select v-model="this.editedItem.student" :items="formattedStudents" item-text="firstName" item-value="lastName" label="Select Student"></v-select>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" variant="text" @click="close">Cancel</v-btn>
            <v-btn color="blue darken-1" variant="text"  @click="save">Save</v-btn>
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
          teachers: [
          {
            firstName: "Mr",
            lastName: "Saci",
          },
          {
            firstName: "Mr",
            lastName: "Ramdani",
          },
          {
            firstName: "Mrs",
            lastName: "Malak",
          }
          ],
          students: [
          {
            firstName: "Kamel",
            lastName: "Mrabet"
          },
          {
            firstName: "Cristiano",
            lastName: "Ronaldo"
          },
          {
            firstName: "Myke",
            lastName: "Tyson"
          },
          ],
          editedItem: {
            Name: "",
            teacher:"",
            student: "",
          },
          defaultItem: {
            Name: "",
            teacher:"",
            student: "",
          }
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
        close() {
          this.dialog = false;
          this.$nextTick(() => {
            this.editedItem = Object.assign({}, this.defaultItem);
          });
        },
        save() {
      if (
        this.editedItem.Name &&
            this.editedItem.teacher &&
            this.editedItem.student
      ) { 
          this.$emit("saveTrain", this.editedItem);
          this.close();
        } else {
        console.error("Error: Please fill out all required fields.");
      }
    },
      }
    };
    </script>
    