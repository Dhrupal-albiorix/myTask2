<template>
  <!-- whole userview -->
  <div class="container">
    <h1>Hello users</h1>
<button @click="form_load">Add New user / search user</button>
    <!-- user form contain starts -->
    <br><br>
    <div class="main">
 

      <!-- form for user starts -->
      <div v-if="formshow">

     <!-- search form starts -->
    
      <!-- search form ends -->
<br><br>
        <form @submit.prevent="AddUser">

        <label for="search">Search:</label>
        <br>
        <input type="text" placeholder="search based on name or email" v-model="search" />
      <br><br>

        <label for="name">User name</label>
        <br />
        <input type="text" v-model="username" required />
        <br /><br />
        <label for="mail">User Email</label>
        <br />
        <input type="email" v-model="mail" />
        <br /><br />
        <label for="phone">phone</label>
        <br />
        <input type="tel" id="phone" name="phone" v-model="phone" />
        <br /><br />
        <label for="dob">Date of Birth</label>
        <br />
        <input type="date" v-model="dob" />
        <br /><br />
        <button :disabled="no_edit ? false : true" @click="update">
          Update
        </button>
        <br /><br />
        <button type="submit" class="btn btn-primary">Add user</button>
      </form>
      </div>
      
      <!-- form for user ends-->
    </div>
    <!-- user form contain ends-->
    <br /><br />
    <h3>user Data</h3>

    <!-- Display userdata table starts -->

    <div>
      <table class="table">
        <tr>
          <th>Number</th>
          <th>name</th>
          <th>email</th>
          <th>mobile</th>
          <th>DOB</th>
          <th>Action</th>
        </tr>

        <tr v-for="(i, index) in filteredData.slice().reverse()" :key="i.id">
          <td>{{ index }}</td>
          <td>{{ i.username }}</td>
          <td>{{ i.mail }}</td>
          <td>{{ i.phone }}</td>
          <td>{{ i.dob }}</td>
          <td>
            <span
              ><button class="btn btn-secondary" @click="edit(index)">
                Edit
              </button></span
            >
            <span
              ><button class="btn btn-secondary" @click="delet(index)">
                Delete
              </button></span
            >
          </td>
        </tr>
      </table>
    </div>
    <!-- Display userdata table ends -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      editingIndex: null, // track the index of the user being edited
      no_edit: false,
      username: "",
      mail: "",
      phone: "",
      dob: "",
      userdata: [],
      search: "", // add search property
      formshow : false
    };
  },

  computed: {
    filteredData() {
      return this.userdata.filter((user) => {  
        return (
          user.username
            .toLowerCase()
            .includes(this.search.toLowerCase().trim()) ||
          user.mail
            .toLowerCase()
            .includes(this.search.toLowerCase().trim()) ||
          user.phone
            .trim()
            .includes(this.search.trim())
        );
      });
    },
  },

  methods: {

    form_load() {
      this.formshow = true
      console.log(this.formshow)
    },

    AddUser() {
      const user = {
        username: this.username,
        mail: this.mail,
        phone: this.phone,
        dob: this.dob,
      };

      if (this.no_edit) {
        // if editing an existing user
        console.log("editing")
        this.userdata.splice(this.editingIndex, 1, user); // replace user at index
        localStorage.setItem("users", JSON.stringify(this.userdata));
      } else {
        // else adding a new user
        this.userdata.push(user);
        localStorage.setItem("users", JSON.stringify(this.userdata));
      }

      this.username = "";
      this.mail = "";
      this.phone = "";
      this.dob = "";
      this.no_edit = false; // reset no_edit after adding/updating user
      this.editingIndex = null; // reset editingIndex
      this.formshow = false
    },

    edit(index) {
       this.formshow = true
      this.editingIndex = index; // store index of user being edited
      const user = this.userdata[index];
      this.username = user.username;
      this.mail = user.mail;
      this.phone = user.phone;
      this.dob = user.dob;
      this.no_edit = true;
    },

    delet(index) {
      this.userdata.splice(index, 1);
      localStorage.setItem("users", JSON.stringify(this.userdata));
    },

    showData() {
      this.userdata = JSON.parse(localStorage.getItem("users")) || [];
    },
  },

  created() {
    this.showData();
  },
};
</script>


<style scoped>
table,
th,
td {
  border: 1px solid black;
}
</style>
