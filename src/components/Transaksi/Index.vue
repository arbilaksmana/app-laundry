<template>
  <div id="wrapper">
    <sidebar-component></sidebar-component>
    <div id="content-wrapper" class="d-flex flex-column">
      <div id="content">
        <navbar-component></navbar-component>
        <div class="container-fluid">
          <h1 class="h3 mb-4 text-gray-800">Data Transaksi</h1>
          <div class="row">
            <div class="col-lg-12">
              <div class="card shadow mb-4">
                <div class="card-body">
                  <div class="table-responsive">
                    <router-link
                      to="/transaksi/tambah"
                      class="btn btn-info btn-icon-split"
                    >
                      <span class="icon text-white-50">
                        <i class="fas fa-plus"></i>
                      </span>

                      <span class="text">Tambah</span>
                    </router-link>
                    <table
                      class="table table bordered mt-3"
                      width="100%"
                      cellspacing="0"
                    >
                      <thead>
                        <tr>
                          <th>#</th>
                          <th>Nama Member</th>
                          <th>Tanggal Order</th>
                          <th>Batas Waktu</th>
                          <th>Tanggal Bayar</th>
                          <th>Status</th>
                          <th>Dibayar</th>
                          <th>Petugas</th>
                          <th>Aksi</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="(t, index) in transaksi" :key="index">
                          <td>{{ index + 1 }}</td>
                          <td>{{ t.nama_member }}</td>
                          <td>{{ t.tgl_order | moment("DD/MM/YYYY") }}</td>
                          <td>{{ t.batas_waktu | moment("DD/MM/YYYY") }}</td>
                          <td>{{ t.tgl_bayar | moment("DD/MM/YYYY") }}</td>
                          <td>
                            <span
                              v-if="t.status == 'baru'"
                              class="badge bg-info text-light"
                              >Baru</span
                            >
                            <span
                              v-if="t.status == 'proses'"
                              class="badge bg-warning text-dark"
                              >Proses</span
                            >
                            <span
                              v-if="t.status == 'selesai'"
                              class="badge bg-success text-light"
                              >Selesai</span
                            >
                            <span
                              v-if="t.status == 'diambil'"
                              class="badge bg-secondary text-light"
                              >Di Ambil</span
                            >
                          </td>
                          <td>
                            <span
                              v-if="t.dibayar == 'dibayar'"
                              class="badge bg-info text-light"
                              >Di Bayar</span
                            >
                            <span
                              v-if="t.dibayar == 'belum dibayar'"
                              class="badge bg-warning text-dark"
                              >Belum Dibayar</span
                            >
                          </td>
                          <td>{{ t.nama }}</td>
                          <!-- <td>{{ t.id }}</td> -->
                          <td>
                            <!-- <router-link
                            type="button"
                            class="btn btn-inverse-primary btn-rounded btn-icon" style="bg-color:blue"
                            :to="{
                              name: 'edittransaksi',
                              params: {
                                id: t.id_transaksi,
                                params: { id: t.id_transaksi },
                              },
                            }"
                          >
                            <i class="icon-settings"></i>
                          </router-link> -->
                            <router-link
                              class="btn btn-success btn-circle"
                              :to="{
                                name: 'detailtransaksi',
                                params: {
                                  id: t.id_transaksi
                                },
                              }"
                            >
                              <i class="fas fa-eye"></i>
                            </router-link>
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
      transaksi: {},
    };
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
      .get("http://localhost/Laundry-UKK-Final/public/api/get_transaksi", {
        headers: { Authorization: "Bearer" + this.$store.state.token },
      })
      .then((res) => {
        this.transaksi = res.data;
      })
      .catch((err) => console.log(err));
  },
  methods: {},
};
</script>
