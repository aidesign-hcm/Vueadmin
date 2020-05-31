<template>
  <section>
    <div class="container vertical-center  mt-4 ">
      <b-col col lg="12"><h2 class="text-center">Add New Product</h2></b-col>
      <b-row class="justify-content-md-center">
        <b-col col lg="9">
            <b-form>
                <div class="form-group">
                <label>Category</label>
                <select class="custom-select" id="input-1" v-model="categoryId">
                    <option
                    v-for="category in categories"
                    :value="category._id"
                    :key="category._id"
                    >{{ category.type }}</option
                    >
                </select>
                </div>
                <b-form>
                    <div class="form-group">
                    <label>Owner</label>
                    <select class="custom-select" id="input-1">
                        <option
                        v-for="owner in owners"
                        :value="owner._id"
                        :key="owner._id"
                        >{{ owner.name }}</option
                        >
                    </select>
                    </div>
                </b-form>
                <b-form-group id="input-group-3" label="Title:" label-for="input-3">
                <b-form-input
                    id="input-3"
                    required
                    placeholder="Enter the title"
                    v-model="title"
                ></b-form-input>
                </b-form-group>

                <b-form-group
                id="input-group-4"
                label="Description:"
                label-for="input-4"
                >
                <b-form-textarea
                    id="input-4"
                    required
                    placeholder="Some thing"
                    v-model="description"
                ></b-form-textarea>
                </b-form-group>

                <b-form-group id="input-group-5" label="Price:" label-for="input-5">
                <b-form-input
                    id="input-5"
                    type="number"
                    required
                    placeholder="price"
                    v-model="price"
                ></b-form-input>
                </b-form-group>

                <b-form-group
                id="input-group-6"
                label="StockQuantity:"
                label-for="input-6"
                >
                <b-form-input
                    id="input-6"
                    type="number"
                    required
                    placeholder="StockQuantity"
                    v-model="StockQuantity"
                ></b-form-input>
                </b-form-group>
                <b-form-group id="input-group-7" label="Photo:" label-for="input-7">
                <b-form-file @change="onFileSelect"></b-form-file>
                {{ fileName }}
                </b-form-group>

                <b-button type="submit" variant="primary" @click="onAddProduct">Submit</b-button>
                <b-button type="reset" variant="danger">Reset</b-button>
            </b-form>
        </b-col>
      </b-row>
    </div>
  </section>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      let categories = $axios.$get("http://localhost:8000/api/categories");
      let owners = $axios.$get("http://localhost:8000/api/owners");
      const [ownerResponse, catResponse] = await Promise.all([
        owners,
        categories
      ]);
      return {
        owners: ownerResponse.owner,
        categories: catResponse.categories
      };
      console.log(categories);
    } catch (error) {
      console.error(error);
    }
  },
  data() {
    return {
        categoryId: null,
        ownerId: null,
        title: "",
        price: 0,
        description: "",
        StockQuantity: 0,
        selectedFile: null,
        fileName: ""
    };
  },
  methods: {
      onFileSelect(event) {
          this.selectedFile = event.target.files[0],
          this.fileName = event.target.files[0].name
      },
      async onAddProduct() {
        try {
          let data = new FormData();
          data.append ("categoryId", this.categoryId);
          data.append ("ownerId", this.ownerId);
          data.append ("title", this.title);
          data.append ("price", this.price);
          data.append ("description", this.description);
          data.append ("StockQuantity", this.StockQuantity);
          data.append ("photo", this.selectedFile, this.selectedFile.name);
          
          let result = await this.$axios.$post('http://localhost:8000/api/products', data);

          this.$router.push('/')
        } catch(err) {
            console.error(error);
        }
        
      }
  }
};
</script>

<style></style>
