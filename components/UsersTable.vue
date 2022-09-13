<template>
  <section>
    <p v-if="loading">loading...</p>
     <div class="text-center my-5">
      <h1>Add permission to a user</h1>
      <div class="input-group d-flex justify-content-center">
        <select v-model="permissionUser.user_id"
          class="custom-select mx-3"
          id="selectUser"
          aria-label="Example select with button addon"
        >
          <option selected>User...</option>
          <option v-for="user in users" :key="user.name" :value="user.id">
            {{ user.name }}
          </option>
        </select>

        <select v-model="permissionUser.permission_id"
          class="custom-select mx-3"
          id="selectPermission"
          aria-label="Example select with button addon"
        >
          <option selected>Permission...</option>
          <option v-for="permission in permissions" :key="permission.permission" :value="permission.id">
            {{permission.permission}}
          </option>
        </select>

        <div class="input-group-append mx-3">
          <button class="btn btn-outline-secondary" type="button" @click="AddPermissionToUser()">
            Add
          </button>
        </div>
      </div>
    </div>

    <table class="table table-striped text-center">
      <thead class="bg-dark text-white">
        <tr>
          <th scope="col" colspan="2">User</th>
          <th scope="col" colspan="4">Permissions</th>
        </tr>
        <tr>
          <td scope="col">Name</td>
          <td scope="col">E-mail</td>
          <td scope="col">Create</td>
          <td scope="col">Update</td>
          <td scope="col">Delete</td>
          <td scope="col">View</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.name">
          <th scope="row">{{ user.name }}</th>
          <td>{{ user.email }}</td>
          <td> X </td>
          <td> X </td>
          <td> X </td>
          <td> X </td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
export default {
  name: "UsersTable",
  data() {
    return {
      loading: Boolean,
      users: Array,
      permissions: Array,
      permissionUser: {
        permission_id: '',
        user_id: ''
      }
    };
  },
  async mounted() {
    try {
      this.loading = true;
      const data = await this.$axios.$get("/api/users");
      console.log(data);
      this.users = data.data;
    } catch (e) {
      console.log(e);
    } finally {
      this.loading = false;
    }

    try {
      this.loading = true;
      const data = await this.$axios.$get("/api/permissions");
      this.permissions = data.data;
    } catch (e) {
      console.log(e);
    } finally {
      this.loading = false;
    }
  },
   methods: {
     async AddPermissionToUser() {
       if(this.permissionUser.permission_id == '' ||  this.permissionUser.user_id == ''){
         return
       }
      try {
        this.loading = true;
        await this.$axios.post('/permission-user', this.permissionUser)
      }catch(error) {
         console.log(error);
      }finally {
        this.loading = false;
      }
    },
  }
};
</script>