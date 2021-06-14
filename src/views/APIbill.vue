<template>
  <div>
    <v-row class="mt-10" no-gutters justify="center">
      <v-col cols="12" sm="6" md="2">
        <div v-for="(item, index) in products" :key="index">
            <v-text-field
              :label="`รหัสสินค้า ${item.productId}`"
              :v-model="bill.numbersOfProduct"
            ></v-text-field>
        </div>
      </v-col>
    </v-row>

    <v-row class="mt-2" no-gutters justify="center">
      <v-col cols="12" sm="3" md="auto">
        <v-btn color="primary" no-gutters justify="center" @click="createBill()"
          >สั่งซื้อ</v-btn
        >
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bill: [],
      i: 0,
      num: 0,
      numsPro: 0,
      bills: [],
      billsDefault: [
        {
          productId: "",
          numbersOfProduct: 0,
        },
      ],
      products: [],
    };
  },
  created() {
    this.initialize();
  },
  methods: {
    initialize() {
      this.getBill();
      this.getProducts();
    },
    async getBill() {
      // การเข้าถึงบิล bill[index].detailProduct[index].{property}
      try {
        var { data } = await this.axios.get("http://localhost:3000/bill/");
        this.bills = data;
        for (this.i in data) {
          this.num++;
        }
        //console.log(this.num);
      } catch (error) {
        console.log(error.message);
      }
    },
    async getProducts() {
      // การเข้าถึงบิล bill[index].detailProduct[index].{property}
      try {
        var { data } = await this.axios.get("http://localhost:3000/stock/");
        this.products = data;
        for (this.i in data) {
          this.numsPro++;
          this.bill[this.i] = {
            productId: data.productId,
            numbersOfProduct: 0,
          };
        }
      } catch (error) {
        console.log(error.message);
      }
    },
    async createBill() {
      try {
        await this.axios.post("http://localhost:3000/bill/", this.bill);
        console.log(this.bill);
        for (this.i in this.numsPro) {
          this.bill[this.i] = {
            numbersOfProduct: 0,
          };
        }
      } catch (error) {
        console.log(error.message);
      }
    },
  },
};
</script>

<style>
</style>