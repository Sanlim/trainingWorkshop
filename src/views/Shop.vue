<template>
  <div>
    <div>
      <v-row no-gutters justify="center">
        <v-card
          v-for="(item, index) in products"
          :key="index"
          :loading="loading"
          class="mx-auto my-6"
          max-width="375"
        >
          <template slot="progress">
            <v-progress-linear
              color="deep-purple"
              height="10"
              indeterminate
            ></v-progress-linear>
          </template>

          <v-img
            height="250"
            :src="`${item.linkPhoto}`"
          />

          <v-card-title>{{ item.productName }}</v-card-title>

          <v-card-text>
            <v-row align="center" class="mx-0"> </v-row>
            <br />
            <div>รายละเอียดสินค้า: {{ item.productDetail }}</div>
          </v-card-text>

          <v-divider class="mx-3"></v-divider>

          <!-- <v-row no-gutters justify="end">
            <v-card-title>สั่งซื้อ</v-card-title>
            <br />
            <v-btn class="mx-3 my-2" fab dark color="green">
              <v-icon medium>mdi-plus</v-icon>
            </v-btn>
          </v-row> -->
          <v-card-actions></v-card-actions>
        </v-card>
      </v-row>

      <!-- dialog -->
      <!--  -->
      <v-row no-gutters justify="center">
        <v-dialog v-model="dialog" persistent max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="mx2"
              color="primary"
              dark
              large
              v-bind="attrs"
              v-on="on"
            >
              <h2>สั่งซื้อ</h2>
            </v-btn>
          </template>

          <!-- dialog สั่งซื้อ -->
          <v-card>
            <v-card-title>
              <span class="text-h4">Bill</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row no-gutters justify="center">
                  <v-col cols="10">
                    <h2 v-for="(item, index) in products" :key="index">
                      {{ item.productName }} <br />
                      <v-text-field
                        class="text-h7"
                        label="จำนวนชิ้น"
                      ></v-text-field>
                    </h2>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red darken-1" large text @click="cancel()">
                ยกเลิก
              </v-btn>
              <v-btn color="blue darken-1" large text @click="save()">
                ตกลง
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      loading: false,
      numProduct: 0,
      countProduct: [],
      dialog: false,
      i: 0,
      count: 0,
      editBill: [
          {
              productId: '',
              numbersOfProduct: 0
          }
      ],
      editBillDefault: [
          {
              productId: '',
              numbersOfProduct: 0
          }
      ],
      link: 'https://sites.google.com/site/dessertofsleeppypillow/_/rsrc/1452222673744/chocolate-1/431467.jpg'
    };
  },
  created() {
    this.getProduct();
  },
  methods: {
    async getProduct() {
      try {
        var { data } = await this.axios.get("http://localhost:3000/stock/");
        this.loading = true;
        setTimeout(() => (this.loading = false), 1000);
        //console.log(data);
        this.products = data;
        for (this.i in data) {
          this.numProduct++;
        }
        this.countProduct = this.numProduct;
        //console.log(this.numProduct);
        // this.show = true;
      } catch (error) {
        console.log(error.message);
      }
    },
    // async getBill() {
    //   try {
    //     var { data } = await this.axios.get("http://localhost:3000/bill/");
    //     this.bill = data
    //     console.log(this.bill);
    //   } catch (error) {
    //     console.log(error.message);
    //   }
    // },
    async createBill() {
      try {
        var { data } = await this.axios.post("http://localhost:3000/bill/", this.editBill);
        console.log(data);
      } catch (error) {
        console.log(error.message);
      }
    },
    save () {
        this.createBill()
        this.editBill = this.editBillDefaul
        this.dialog = false
    },
    cancel () {
        this.editBill = this.editBillDefault
        this.dialog = false
    },
  },
};
</script>

<style scoped>
</style>