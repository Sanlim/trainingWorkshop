<template>
  <v-data-table
    :headers="headers"
    :items="products"
    sort-by="productId"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>My Stock</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
              New Item
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.productId"
                      label="รหัสสินค้า"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.productName"
                      label="ชื่อสินค้า"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.productDetail"
                      label="รายละเอียดสินค้า"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.productPrice"
                      label="ราคาสินค้า"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.currentProduct"
                      label="จำนวนคงเหลือ"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.linkPhoto"
                      label="ลิงค์รูปภาพ"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red darken-1" text @click="close"> Cancel </v-btn>
              <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5"
              >คุณต้องการลบรายการสินค้านี้ใช่หรือไม่?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red darken-1" text @click="closeDelete"
                >Cancel</v-btn
              >
              <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
      <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary"> Reset </v-btn>
    </template>
  </v-data-table>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "รหัสสินค้า",
        align: "start",
        value: "productId",
      },
      { text: "ชื่อสินค้า", value: "productName" },
      { text: "รายละเอียดสินค้า", value: "productDetail", sortable: false },
      { text: "ราคาสินค้า", value: "productPrice" },
      { text: "จำนวนคงเหลือ", value: "currentProduct" },
      { text: "Actions", value: "actions", sortable: false },
    ],
    products: [],
    test: [],
    test2: [],
    editedIndex: -1,
    editedItem: {
      productId: "",
      productName: "",
      productDetail: "",
      productPrice: 0,
      currentProduct: 0,
      linkPhoto: "",
    },
    defaultItem: {
      productId: "",
      productName: "",
      productDetail: "",
      productPrice: 0,
      currentProduct: 0,
      linkPhoto: "",
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.getProduct();
    },

    editItem(item) {
      this.editedIndex = this.products.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.products.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      //console.log(ite);
      this.delProduct()
      this.products.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },
    save() {
      if (this.editedIndex > -1) {
        this.editProduct();
        Object.assign(this.products[this.editedIndex], this.editedItem);
      } else {
        //this.getProduct()
        this.createProduct();
        this.products.push(this.editedItem);
      }
      this.close();
    },
    async getProduct() {
      try {
        var { data } = await this.axios.get("http://localhost:3000/stock/");
        //console.log(data);
        this.products = data;
        // this.show = true;
      } catch (error) {
        console.log(error.message);
      }
    },
    async createProduct() {
      try {
        await this.axios.post("http://localhost:3000/stock/", this.editedItem);
        //console.log(data);
        this.products = this.getProduct();
        // this.show = true;
      } catch (error) {
        console.log(error.message);
      }
    },
    async editProduct() {
      try {
        await this.axios.put("http://localhost:3000/stock/", this.editedItem);
        //console.log(data);
        this.products = this.getProduct();
      } catch (error) {
        console.log(error.message);
      }
    },
    async delProduct() {
      try {
        // console.log(item);
        await this.axios.delete("http://localhost:3000/stock/" + this.editedItem)
        this.getProduct()
      } catch (error) {
        console.log(error.message);
      }
    },
  },
};
</script>