<style>
.widget-user-header {
  background-position: center center;
  background-size: cover;
  height: 250px !important;
}
.widget-user .card-footer {
  padding: 0;
}
</style>


<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-12 mt-3">
        <div class="card card-widget widget-user">
          <!-- Add the bg color to the header using any of the bg-* classes -->
          <div
            class="widget-user-header text-white"
            style="background-image:url('./img/user-cover.jpg')"
          >
            <h3 class="widget-user-username">Name</h3>
            <h5 class="widget-user-desc">Type</h5>
          </div>
          <div class="widget-user-image">
            <img class="img-circle" src alt="User Avatar">
          </div>
          <div class="card-footer">
            <div class="row">
              <div class="col-sm-4 border-right">
                <div class="description-block">
                  <h5 class="description-header">3,200</h5>
                  <span class="description-text">SALES</span>
                </div>
                <!-- /.description-block -->
              </div>
              <!-- /.col -->
              <div class="col-sm-4 border-right">
                <div class="description-block">
                  <h5 class="description-header">13,000</h5>
                  <span class="description-text">FOLLOWERS</span>
                </div>
                <!-- /.description-block -->
              </div>
              <!-- /.col -->
              <div class="col-sm-4">
                <div class="description-block">
                  <h5 class="description-header">35</h5>
                  <span class="description-text">PRODUCTS</span>
                </div>
                <!-- /.description-block -->
              </div>
              <!-- /.col -->
            </div>
            <!-- /.row -->
          </div>
        </div>
      </div>
      <div class="col-md-9">
        <div class="card">
          <div class="card-header p-2">
            <ul class="nav nav-pills">
              <li class="nav-item">
                <a class="nav-link" href="#activity" data-toggle="tab">Activity</a>
              </li>
              <li class="nav-item">
                <a class="nav-link active show" href="#settings" data-toggle="tab">Settings</a>
              </li>
            </ul>
          </div>
          <!-- /.card-header -->
          <div class="card-body">
            <div class="tab-content">
              <div class="tab-pane" id="activity">
                <h3 class="text-center">Display User Activity</h3>
              </div>
              <!-- /.tab-pane -->
              <!-- /.tab-pane -->
              <div class="tab-pane active show" id="settings">
                <form class="form-horizontal">
                  <div class="form-group">
                    <label for="inputName" class="col-sm-12 control-label">Name</label>

                    <div class="col-sm-12">
                      <input
                        v-model="form.name"
                        type="text"
                        name="name"
                        placeholder="Name"
                        class="form-control"
                      >
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="inputEmail" class="col-sm-12 control-label">Email</label>

                    <div class="col-sm-12">
                      <input
                        type="text"
                        v-model="form.email"
                        name="email"
                        placeholder="Email Address"
                        class="form-control"
                      >
                    </div>
                  </div>

                  <div class="form-group">
                    <label for="inputExperience" class="col-sm-12 control-label">Experience</label>

                    <div class="col-sm-12">
                      <textarea id="inputExperience" placeholder="Experience" class="form-control"></textarea>
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="photo" class="col-sm-12 control-label">Profile Photo</label>
                    <div class="col-sm-offset-2 col-sm-12">
                      <input type="file" @change="updateProfile" name="photo" class="form-input">
                    </div>
                  </div>
                  <div class="form-group">
                    <label
                      for="password"
                      class="col-sm-12 control-label"
                    >Passport (leave empty if not changing)</label>

                    <div class="col-sm-12">
                      <input
                        type="password"
                        class="form-control"
                        id="password"
                        placeholder="Passport"
                      >
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="col-sm-offset-2 col-sm-12">
                      <button
                        @click.prevent="updateInfo"
                        type="submit"
                        class="btn btn-success"
                      >Update</button>
                    </div>
                  </div>
                </form>
              </div>
              <!-- /.tab-pane -->
            </div>
            <!-- /.tab-content -->
          </div>
          <!-- /.card-body -->
        </div>
        <!-- /.nav-tabs-custom -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: new Form({
        id: "",
        name: "",
        email: "",
        password: "",
        type: "",
        bio: "",
        photo: ""
      })
    };
  },
  mounted() {
    console.log("Component mounted.");
  },
  methods: {
    updateInfo() {
      this.$Progress.start();
      // console.log("Editing Data");
      this.form
        .put("api/profile")
        .then(() => {
          // success
          this.$Progress.finish();
        })
        .catch(() => {
          this.$Progress.fail();
        });
    },
    updateProfile(element) {
      let file = element.target.files[0];
      let reader = new FileReader();

      if (file.size < 211775) {
        reader.onloadend = file => {
          this.form.photo = reader.result;
        };
        reader.readAsDataURL(file);
      } else {
        Swal.fire("Oops...", "You are uploading a large file", "error");
      }
    }
  },
  created() {
    axios.get("api/profile").then(({ data }) => this.form.fill(data));
  }
};
</script>
