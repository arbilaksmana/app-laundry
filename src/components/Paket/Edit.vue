<template>
  <div id="wrapper">
    <sidebar-component></sidebar-component>
    <div id="content-wrapper" class="d-flex flex-column">
      <div id="content">
        <navbar-component></navbar-component>
        <div class="container-fluid">
          <div class="row">
            <div class="col-lg-8">
              <div class="card shadow mb-4">
                <div class="card-header py-3">
                  <h6 class="m-0 font-weight-bold text-primary">
                    Tambah Data Paket
                  </h6>
                </div>
                <div class="card-body">
                  <form @submit.prevent="tambah" class="forms-sample">
                    <div class="form-group row">
                      <label
                        for="exampleInputUsername2"
                        class="col-sm-3 col-form-label"
                        >Jenis Paket</label
                      >

                      <div class="col-sm-12 col-md-6">
                        <input type="text" v-model="paket.jenis"
                          class="form-control"
                          aria-label="Default select example" disabled>

                        <!-- <select
                          
                        >
                          <option value="Cuci setrika">Cuci setrika</option>
                          <option value="Cuci kering">Cuci kering</option>
                          <option value="Sprei kecil">Sprei kecil</option>
                          <option value="Sprei Besar">Sprei Besar</option>
                          <option value="Bed cover">Bed cover</option>
                        </select> -->
                      </div>

                      <!-- <div class="col-sm-12 col-md-6">
                        <div class="dataTables_length" id="dataTable_length">
                          <label 
                            >
                            <select
                              name="dataTable_length"
                              class="
                                form-select
                                custom-select custom-select-sm
                                form-control form-control-sm
                              "
                              v-model="paket.jenis"
                              aria-label="Default select example"
                              
                              
                            >
                            
                              <option selected>Open this select menu</option>
                              <option value="Cuci setrika">Cuci setrika</option>
                              <option value="Cuci kering">Cuci kering</option>
                              <option value="Sprei kecil">Sprei kecil</option>
                              <option value="Sprei Besar">Sprei Besar</option>
                              <option value="Bed cover">Bed cover</option>
                              
                            </select>
                        </label
                          >
                        </div>
                      </div> -->
                    </div>
                    <div class="form-group row">
                      <label
                        for="exampleInputUsername2"
                        class="col-sm-3 col-form-label"
                        >Harga</label
                      >
                      <div class="col-sm-9">
                        <input
                          type="text"
                          class="form-control"
                          id="exampleInputUsername2"
                          placeholder="Masukan Harga"
                          v-model="paket.harga"
                        />
                      </div>
                    </div>

                    <!-- <div class="col-sm-12 col-md-6">
                      <div class="dataTables_length" id="dataTable_length">
                        <label
                          >Show
                          <select
                            name="dataTable_length"
                            aria-controls="dataTable"
                            class="
                              custom-select custom-select-sm
                              form-control form-control-sm
                            "
                          >
                            <option value="10">10</option>
                            <option value="25">25</option>
                            <option value="50">50</option>
                            <option value="100">100</option>
                          </select>
                          entries</label
                        >
                      </div>
                    </div> -->
                    <!-- <div class="dropdown mb-4">
                      <button
                        class="btn btn-secondary dropdown-toggle"
                        type="button"
                        id="dropdownMenuButton"
                        data-toggle="dropdown"
                        aria-haspopup="true"
                        aria-expanded="false"
                      >
                        Dropdown 
                      </button>
                      <div
                        class="dropdown-menu"
                        aria-labelledby="dropdownMenuButton"
                        style=""
                      >
                        <a class="dropdown-item" href="#">Action</a>
                        <a class="dropdown-item" href="#">Another action</a>
                        <a class="dropdown-item" href="#"
                          >Something else here</a
                        >
                      </div>
                    </div> -->

                    <!-- <div class="form-check form-check-flat form-check-primary">
                      <label class="form-check-label">
                        <input type="checkbox" class="form-check-input" />
                        Remember me
                      </label>
                    </div> -->
                    <button type="submit" class="btn btn-primary mr-2">
                      Submit
                    </button>
                    <button class="btn btn-light">Cancel</button>
                  </form>
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
      paket: {},
    };
  },
  created() {
    var data = JSON.parse(this.$store.state.datauser);
    var role = data.role;
    if (role == "kasir" || role == "owner") {
      this.$swal("Anda tidak dapat mengakses halaman ini");
      this.$router.push("/");
    }
    this.axios
      .get(
        `http://localhost/Laundry-UKK-Final/public/api/get_paket_id/${this.$route.params.id_paket}`,
        {
          headers: { Authorization: "Bearer " + this.$store.state.token },
        }
      )
      .then((res) => {
        this.paket = res.data.data.paket;
        console.log(res.data.data.paket);
      })
      .catch((err) => console.log(err));
  },
  methods: {
    tambah() {
      this.axios
        .put(
          `http://localhost/Laundry-UKK-Final/public/api/update_paket/${this.$route.params.id_paket}`,
          this.paket,
          {
            headers: { Authorization: "Bearer" + this.$store.state.token },
          }
        )
        .then(() => {
          this.$router.push("/paket");
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>