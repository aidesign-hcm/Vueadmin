<template>
  <section>
    <div class="container vertical-center  mt-4 ">
      <b-col col lg="12"><h2 class="text-center">Add New Category</h2></b-col>
      <b-row class="justify-content-md-center">
        <b-col col lg="9">
          <b-form>
            <b-form-group id="input-group" label="Type:">
              <b-form-input
                required
                placeholder="Enter the type"
                v-model="type"
              ></b-form-input>
            </b-form-group>
            <span class="btn btn-primary" @click="onAddCategory"
              >Submit</span>
          </b-form>
        </b-col>
      </b-row>
      <div class="row">
          <div class="col-lg-9 mx-auto">
              <ul class="list-group mt-3 mx-0">
                  <li class="list-group-item" v-for="cat in categories" :key="cat._id">{{cat.type}}</li>
              </ul>
          </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
    async asyncData ({ $axios }) {
    try {
      const response = await $axios.$get('http://localhost:8000/api/categories');
      return {
        categories: response.categories    
      }
    } catch (error) {
      console.log(error);
    }
  },
    data() {
    return {
        type: "",
        };
    },
    methods: {
      async onAddCategory() {
        try {
        let data = {type: this.type};
            let result = await this.$axios.$post('http://localhost:8000/api/categories', data);
            this.categories.push(data)
        } catch(err) {
                console.log(error);
        }
    }
  }
}
</script>

<style></style>
