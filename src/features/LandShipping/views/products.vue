<template>
  <div>
    <v-card class="pa-5">
      <v-btn icon @click="$emit('close')">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-dialog v-model="addProduct" width="900" persistent>
        <AddEditProduct
          @closeDilog="addProduct = false"
          :myProducts="myProducts"
          :productData="productData"
          :categoryData="categoryData"
          :resturantId="resturantId"
          @EditProduct="EditArr($event, productData)"
          @AddCategory="AddToArr($event, productData)"
          :ingeradients="ingeradients"
        ></AddEditProduct>
      </v-dialog>
      <div class="d-flex justify-content-right mt-10 mb-5">
        <AddBtn :content="'إضافة اكلة'" @submit="(addProduct = true), (myProducts = {})"></AddBtn>
      </div>
      <productTable
        v-if="productData"
        :productData="productData"
        @openDilog="addProduct = true"
        @myProductsEdit="myProducts = $event"
        @productId="DeleteObjFromArr(productData, $event)"
        :loadingMain="loadingMainData"
        :ingeradients="ingeradients"
      ></productTable>
    </v-card>
  </div>
</template>
<script>
import productTable from "./components/productTable.vue";
import productsApi from "../services/productsApi";
import AddEditProduct from "./components/AddEditProduct.vue";
export default {
  props: ["Resturantid", "selecteditem"],
  components: {
    productTable,
    AddEditProduct
  },
  data() {
    return {
      addProduct: false,
      productData: [],
      myProducts: {},
      categoryData: [],
      resturantId: null,
      loading: false,
      products: [],
      images: null,
      imagesProduct: [],
      openDeleteProductImg: false,
      ingeradients: []
    };
  },
  async mounted() {
    try {
      this.resturantId = this.Resturantid;
      this.loadingMainData = true;
      const res = await productsApi.getProducts(this.Resturantid);
      console.log(res);
      this.productData = res.data.products;
      const ress = await productsApi.getCategories(this.Resturantid);
      this.categoryData = ress.data.categoryies;
      console.log(ress);
      const ingeradient = await productsApi.getIngeradient();
      console.log(ingeradient);
      this.ingeradients = ingeradient.data.ingredients;
      this.loadingMainData = false;
    } catch (error) {
      console.log(error);
      this.loadingMainData = false;
    }
  },
  methods: {}
};
</script>
<style lang="scss" scoped></style>
