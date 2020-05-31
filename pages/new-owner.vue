<template>
  <section>
    <div class="container vertical-center  mt-4 ">
      <b-col col lg="12"><h2 class="text-center">Add New Owner</h2></b-col>
      <b-row class="justify-content-md-center">
        <b-col col lg="9">
          <b-form>
            <b-form-group id="input-group" label="name:">
              <b-form-input
                required
                placeholder="Enter the name"
                v-model="name"
              ></b-form-input>
            </b-form-group>

            <b-form-group label="About:">
              <b-form-textarea
                required
                placeholder="Some thing"
                v-model="about"
              ></b-form-textarea>
            </b-form-group>
            <b-form-group id="input-group-7" label="Photo:" label-for="input-7">
              <b-form-file @change="onFileSelect"></b-form-file>
              {{ fileName }}
            </b-form-group>
            <button type="submit" class="btn btn-primary" @click="onAddOwner">
              Submit
            </button>
          </b-form>
        </b-col>
      </b-row>

      <div class="row">
        <div class="col-lg-9 mx-auto">
          <b-list-group class="my-3" style="max-width: 300px;">
            <b-list-group-item class="d-flex align-items-center"  v-for="own in owners" :key="own._id">
              <b-avatar class="mr-3" :src="own.photo"></b-avatar>
              <span class="mr-auto">{{ own.name }}</span>
              <span>{{ own.about }}</span>
            </b-list-group-item>
          </b-list-group>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      const response = await $axios.$get("http://localhost:8000/api/owners");
      console.log(response.owner);
      return {
        owners: response.owner
      };
    } catch (error) {
      console.log(error);
    }
  },
  data() {
    return {
      name: "",
      about: "",
      selectedFile: null,
      fileName: ""
    };
  },
  methods: {
    onFileSelect(event) {
      (this.selectedFile = event.target.files[0]),
        (this.fileName = event.target.files[0].name);
    },
    async onAddOwner() {
      try {
        let data = new FormData();
        data.append("name", this.name);
        data.append("about", this.about);
        data.append("photo", this.selectedFile, this.selectedFile.name);
        let result = await this.$axios.$post(
          "http://localhost:8000/api/owners",
          data
        );
        this.owners.push(this.name);
      } catch (err) {
        console.log(error);
      }
    }
  }
};
</script>

<style></style>
