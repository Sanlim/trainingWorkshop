<template>
  <div class="d-flex justify-center mb-6">

      <v-app-bar app color="primary" dark>
        <div class="d-flex align-center">
          <!-- <v-img
            alt="Vuetify Logo"
            class="shrink mr-2"
            contain
            transition="scale-transition"
            width="55"
          /> -->

          <v-btn to="/" text>
            <h1>Shop</h1>
          </v-btn>
          
          <v-btn to="/bill" text>
            <h2 class="mr-2">บิล</h2>
            <v-icon>mdi-note-plus</v-icon>
          </v-btn>
        </div>

        <v-spacer></v-spacer>

        <!-- <v-btn text>
          <span class="mr-2">Home</span>
          <v-icon>mdi-home</v-icon>
        </v-btn> -->
        <v-btn to="/product" text>
          <h2 class="mr-2">สินค้า</h2>
          <v-icon>mdi-database</v-icon>
        </v-btn>
        <!-- <v-btn to="about" text>
          <span class="mr-2">About</span>
          <v-icon>mdi-open-in-new</v-icon>
        </v-btn> -->
      </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </div>
</template>

<script>
export default {};
</script>
