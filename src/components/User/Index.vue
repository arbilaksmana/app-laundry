<template>
    <div id="wrapper">
        <sidebar-component></sidebar-component>
        <div id="content-wrapper" class="d-flex flex-column">
            <div id="content">
                <navbar-component></navbar-component>

                <div class="container-fluid">
                    <!-- Page Heading -->
                    <h1 class="h3 mb-4 text-gray-800">Data User</h1>

                    <div class="row">
                        <div class="col-lg-12">
                            <div class="card shadow mb-4">
                                <div class="card-body">
                                    <div class="table-responsive">
                                        <router-link to="/user/tambah" class="btn btn-info btn-icon-split">
                                            <span class="icon text-white-50">
                                                <i class="fas fa-plus"></i>
                                            </span>
                                            <span class="text">Tambah</span>
                                        </router-link>
                                        <table class="table table-bordered mt-3" width="100%" cellspacing="0">
                                            <thead>
                                                <th>#</th>
                                                <th>Nama</th>
                                                <th>Username</th>
                                                <th>Role</th>
                                                <th>Outlet</th>
                                                <th>Aksi</th>
                                            </thead>
                                            <tbody>
                                                <tr v-for="(u, index) in users" :key="index">
                                                    <td>{{ index + 1 }}</td>
                                                    <td>{{ u.nama }}</td>
                                                    <td>{{u.username}}</td>
                                                    <td>{{ u.role }}</td>
                                                    <td>{{ u.id_outlet }}</td>
                                                    <td>
                                                       <router-link class="btn btn-warning btn-circle" :to="{ name : 'edituser' , params : { id : u.id } }"><i class="fas fa-pen"></i></router-link>
                                                       <button type="button" @click="hapus(u.id)" class="btn btn-danger btn-circle"><i class="fas fa-trash"></i></button>
                                                   </td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <footer-component></footer-component>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            users : {}
        }
    },
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        var role = data.role

        if(role == 'kasir' || role == 'owner')
        {
            this.$swal("Anda tidak dapat mengakses halaman ini")
            this.$router.push('/') 
        }
        
        this.axios.get('http://localhost/Laundry-UKK-Final/public/api/get_user', { headers : { 'Authorization' : 'Bearer ' + this.$store.state.token} })
                  .then((res) => {
                      this.users = res.data
                  })
                  .catch(err => console.log(err))
    },
    methods : {
        alertDisplay(id_user) {
      this.$swal({
        title: "Are you sure?",
        text: "You can't revert your action",
        type: "warning",
        showCancelButton: true,
        confirmButtonText: "Yes Delete it!",
        cancelButtonText: "No, Keep it!",
        showCloseButton: true,
        showLoaderOnConfirm: true,
      }).then((result) => {
        if (result.value) {
          this.$swal(
            "Deleted",
            "You successfully deleted this file",
            "success",
            this.axios
              .delete(
                `http://localhost/Laundry-UKK-Final/public/api/delete_member/${id_user}`,
                {
                  headers: {
                    Authorization: `Bearer ` + this.$store.state.token,
                  },
                }
              )
              .then(() => {
                let i = this.user
                  .map((item) => item.id_user)
                  .indexOf(id_user);
                this.user.splice(i, 1);
              })
          );
        } else {
          this.$swal("Cancelled", "Your Data Is Still Intact", "info");
        }
      });
    },
        hapus(id) {
            this.axios.delete(`http://localhost/Laundry-UKK-Final/public/api/delete_user/${id}`, { headers : { 'Authorization' : 'Bearer ' + this.$store.state.token} })
                      .then(() => {
                          let i = this.users.map(item => item.id).indexOf(id);
                          this.users.splice(i, 1)
                      })
                      .catch(err => console.log(err))
        }
    }
}
</script>