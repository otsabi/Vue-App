<template>
  <b-container fluid>
    <b-row>
      <b-col offset="4" md="4">
        <v-select v-model="categoryFilter" :options="categorys" class="w-100" />
      </b-col>
      <b-col md="4"> </b-col>
    </b-row>
    <b-row class="justify-content-md-center">
      <b-col md="12">
        <p class="mt-3">Current Page: {{ currentPage }}</p>

        <b-table
          responsive
          :items="filterProducts"
          :fields="fields"
          :per-page="perPage"
          :current-page="currentPage"
          class="m-4"
        >
          <template #cell(image)="data">
            <b-avatar variant="info" :src="data.item.image"></b-avatar>
          </template>

          <template #cell(rating)="data">
            <b-form-rating v-model="data.item.rating.rate"></b-form-rating>
            {{ data.item.rating.count }}
          </template>
        </b-table>
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          class="mt-4"
        >
          <template #first-text
            ><span class="text-success">First</span></template
          >
          <template #prev-text><span class="text-danger">Prev</span></template>
          <template #next-text><span class="text-warning">Next</span></template>
          <template #last-text><span class="text-info">Last</span></template>
          <template #ellipsis-text>
            <b-spinner small type="grow"></b-spinner>
            <b-spinner small type="grow"></b-spinner>
            <b-spinner small type="grow"></b-spinner>
          </template>
          <template #page="{ page, active }">
            <b v-if="active">{{ page }}</b>
            <i v-else>{{ page }}</i>
          </template>
        </b-pagination>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
// @ is an alias to /src
import {
  BContainer,
  BRow,
  BCol,
  BTable,
  BPagination,
  BAvatar,
  BFormRating,
} from "bootstrap-vue";
import vSelect from "vue-select";
import axios from "axios";
export default {
  name: "Home",
  components: {
    BContainer,
    BRow,
    BCol,
    BTable,
    BPagination,
    vSelect,
    BAvatar,
    BFormRating,
  },
  data() {
    return {
      fields: [
        { key: "id" },
        { key: "description" },
        { key: "image" },
        { key: "title" },
        { key: "category" },
        {
          key: "price",
          sortable: true,
        },
        {
          key: "rating",
          sortable: true,
        },
      ],
      categoryFilter: null,
      categoryOptions: null,
      products: [],
      categorys: [],
      perPage: 5,
      currentPage: 1,
    };
  },
  computed: {
    rows() {
      return this.products.length;
    },
    filterProducts() {
      if (this.categoryFilter) {
        return this.products.filter(
          (product) => product.category == this.categoryFilter
        );
      } else {
        return this.products;
      }
    },
  },
  async created() {
    try {
      const response = await axios.get("https://fakestoreapi.com/products");
      this.products = response.data;

      this.products.forEach((product) => {
        if (this.categorys.indexOf(product.category) === -1) {
          this.categorys.push(product.category);
        }
      });
    } catch (error) {
      console.error(error);
    }
  },
  watch: {},
};
</script>
