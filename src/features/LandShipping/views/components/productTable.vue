<template>
  <div>
    <template>
      <v-card>
        <v-card-title>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="بحث"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table
          :loading="loadingMain"
          :headers="headersProduct"
          :items="productData"
          :search="search"
        >
          <template v-slot:[`item.img`]="{ item }">
            <img width="100px" height="100px" :src="item.img[0]" class="pa-2" />
          </template>
          <template v-slot:[`item.size`]="{ item }">
            <v-chip :color="getColor(item.size)" dark>
              <p class="ma-0 ">{{ item.size }}</p>
            </v-chip>
          </template>
          <template v-slot:[`item.ingredients`]="{ item }">
            <v-card class="pt-2">
              <h2 class="mb-0 text-capitalize">explore from here</h2>

              <v-spacer></v-spacer>

              <v-menu bottom left>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn dark icon v-bind="attrs" v-on="on">
                    <v-icon color="black">mdi-arrow-down-thin</v-icon>
                    <!-- <v-icon color="black">mdi-arrow-down-circle</v-icon> -->
                  </v-btn>
                </template>

                <v-list>
                  <v-list-item v-for="(ingredient, index) in item.ingredients" :key="index">
                    <v-list-item-avatar>
                      <img :src="ingredient.img" alt="John" />
                    </v-list-item-avatar>
                    <v-list-item-title>{{ ingredient.name }}</v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-menu>
            </v-card>
          </template>
          <template v-slot:[`item.actions`]="{ item }">
            <v-icon class="ml-2" color="success" medium @click="editProduct(item)">mdi-pen</v-icon>
            <v-icon color="error" medium @click="opendelProduct(item)">mdi-delete</v-icon>
          </template>
        </v-data-table>
        <v-dialog v-model="openDeleteProduct" width="500">
          <deleteItemsConfirmMsg
            :loading="loading"
            @submit="delProduct"
            @closeDeleteDilog="openDeleteProduct = false"
            content="الاكلة"
          ></deleteItemsConfirmMsg>
        </v-dialog>
      </v-card>
    </template>
  </div>
</template>

<script>
import productsApi from "../../services/productsApi";
export default {
  props: ["productData", "loadingMain"],
  data() {
    return {
      headersProduct: [
        {
          text: "الاسم",
          align: "center",
          value: "name"
        },
        {
          text: "الوصف",
          align: "center",
          value: "description"
        },
        {
          text: "السعر",
          align: "center",
          value: "price"
        },
        {
          text: "العدد",
          align: "center",
          value: "quantity"
        },
        {
          text: "الحجم",
          align: "center",
          value: "size"
        },
        {
          text: "المكونات",
          align: "center",
          value: "ingredients"
        },
        {
          text: "الصورة",
          align: "center",
          value: "img"
        },

        {
          text: "تعديل او حذف",
          align: "center",
          value: "actions"
        }
      ],
      search: "",
      openDeleteProduct: false,
      deletedProductId: null,
      loading: false
    };
  },
  methods: {
    getColor(item) {
      if (item === 3) {
        return "red";
      } else if (item === 1) {
        return "green";
      }
    },
    editProduct(item) {
      this.$emit("openDilog");
      this.$emit("myProductsEdit", { ...item });
    },
    opendelProduct(item) {
      console.log("sss");
      this.openDeleteProduct = true;
      this.deletedProductId = item._id;
    },
    async delProduct() {
      try {
        this.loading = true;
        const res = await productsApi.deleteProduct(this.deletedProductId);
        console.log(res);
        this.loading = false;
        this.openDeleteProduct = false;
        this.$emit("productId", this.deletedProductId);
        this.ToasteSuccessMsg("تم مسح الاكلة بنجاح");
      } catch (err) {
        console.log(err);
        this.loading = false;
        this.toastErrorMsg(
          error.response.data.msgs.ar ||
            error.response.data.errors.msgs.ar ||
            "حدث خطأ اثناء مسح الاكلة"
        );
      }
    }
  },
  // mounted() {
  //   this.productData.forEach(e => {
  //     if (e.size === 3) {
  //       e.size = "bigs";
  //     }
  //   });
  // }
  watch: {
    productData(newData, oldData) {
       this.productData.forEach(e => {
      if (e.size === 3) {
        e.size = "small";
      }else if (e.size === 2) {
         e.size = "medium";
      }else if (e.size === 1) {
         e.size = "Big";
      }
      
    });
    }
  }
};
</script>

<style lang="scss" scoped></style>
