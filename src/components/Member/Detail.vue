<template>
  <div id="wrapper">
    <sidebar-component></sidebar-component>
    <div id="content-wrapper" class="d-flex flex-column">
      <div id="content">
        <navbar-component></navbar-component>
        <div class="container-fluid">
          <h1 class="h3 mb-4 text-gray-800">Data Member</h1>
          <div class="row">
            <div class="col-lg-12">
              <div class="card shadow mb-4">
                <div class="card-body">
                  <div class="d-sm-flex align-items-center mb-4">
                    <h4 class="card-title mb-sm-0">Our Member</h4>
                  </div>
                  <div class="row align-items-center">
                    <div class="col-md-4 text-center">
                      <i class="far fa-user fa-7x text-gray-300"></i>
                    </div>
                    <div class="col py-2">
                      <div
                        class="
                          text-xs
                          font-weight-bold
                          text-primary text-uppercase
                        "
                      >
                        Nama
                      </div>
                      <div class="h6 mb-0 font-weight-bold text-gray-800 mb-2">
                        {{ member.nama_member }}
                      </div>
                      <div
                        class="
                          text-xs
                          font-weight-bold
                          text-primary text-uppercase
                        "
                      >
                        Alamat
                      </div>
                      <div class="h6 mb-0 font-weight-bold text-gray-800 mb-2">
                        {{ member.alamat }}
                      </div>
                      <div
                        class="
                          text-xs
                          font-weight-bold
                          text-primary text-uppercase
                        "
                      >
                        Jenis Kelamin
                      </div>
                      <div class="h6 mb-0 font-weight-bold text-gray-800 mb-2">
                        {{ member.jenis_kelamin }}
                      </div>
                      <div
                        class="
                          text-xs
                          font-weight-bold
                          text-primary text-uppercase
                        "
                      >
                        Telepon
                      </div>
                      <div class="h6 mb-0 font-weight-bold text-gray-800 mb-2">
                        {{ member.tlp }}
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- <footer-component></footer-component> -->
  </div>
</template>
<script>
export default {
  data() {
    return {
      id : this.$route.params.id,
      member: {}
    }
  },
  created() {
    var data = JSON.parse(this.$store.state.datauser);
    var role = data.role;

    if (role == "owner") 
    {
      this.$swal("Anda tidak dapat mengakses halaman ini");
      this.$router.push("/");
    }

    this.axios
      .get(
        `http://localhost/Laundry-UKK-Final/public/api/get_member_id/${this.$route.params.id}`,
        { headers: { Authorization: "Bearer" + this.$store.state.token } }
      )
      .then((res) => {
        this.member = res.data;
      })
      .catch((err) => console.log(err));
  },
};
</script>