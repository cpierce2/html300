<!-- Main content -->
<template>
  <!-- creates the card that holds the tabs and pages -->
  <v-card>
    <!-- nested card -->
    <v-card>
      <!-- toolbar -->
      <v-toolbar>
        <!-- menu for toolbar -->
        <v-menu open-on-hover>
          <!-- slot for buttons -->
          <template v-slot:activator="{ props }">
            <!-- icon button -->
            <v-btn 
            icon='mdi-account'
            v-bind='props' />
          </template>
            
          <!-- creates list -->
          <v-list>
            <!-- list item -->
            <v-list-item>
              <!-- card -->
              <v-card
                class="mx-auto"
                elevation="1"
                max-width="500">

                <!-- card title -->
                <v-card-title class="py-5 font-weight-black">Golden Shine Log In</v-card-title>
                <!-- card text -->
                <v-card-text>
                  This login section is intentionally not operational.
                </v-card-text>
                <!-- card text -->
                <v-card-text>
                  <!-- username -->
                  <div class="text-subtitle-2 font-weight-black mb-1">Enter Username</div>
                  <!--  user email text field -->
                  <v-text-field
                    label="Username@email.com"
                    single-line
                    variant="outlined"
                  />
                  <!-- log in button -->
                  <v-btn
                    :disabled="loading"
                    :loading="loading"
                    block
                    class="text-none mb-4"
                    color="indigo-darken-3"
                    size="x-large"
                    variant="flat"
                    @click="loading = !loading">
                    Log In
                  </v-btn>

                  <!-- button to cancel operation -->
                  <v-btn
                    block
                    class="text-none"
                    color="grey-lighten-3"
                    size="x-large"
                    variant="flat">
                    Cancel
                  </v-btn>
                </v-card-text>
              </v-card>
            </v-list-item>
          </v-list>
        </v-menu>

        <!-- spacer -->
        <v-spacer />

        <!-- card text -->
        <v-card-text>
          <!-- text field -->
          <v-text-field
            :loading="loading"
            density="compact"
            variant="solo"
            label="Search Golden Shine Jewlery Shop"
            append-inner-icon="mdi-magnify"
            single-line
            hide-details
            @click:append-inner="onClick" 
          />
        </v-card-text>

        <!-- spacer -->
        <v-spacer />

        <!-- dialog window -->
        <v-dialog>
          <!-- slot for icon -->
          <template v-slot:activator="{ props }">
            <!-- icon button -->
            <v-btn v-bind="props" icon='mdi-cart-variant' />
          </template>

          <!-- slot to append vue page views -->
          <template v-slot:default="{ isActive }">
            <!-- creates the card that displays the content -->
            <v-card>
              <!-- card text -->
              <v-card-text>
                <!-- imported cart -->
                <cart />
              </v-card-text>

              <!-- divider -->
              <v-divider />

              <!-- creates card actions -->
              <v-card-actions>
                <!-- spacer -->
                <v-spacer />
                <!-- button to close dialog window -->
                <v-btn icon='mdi-close' @click='isActive.value = false' />
              </v-card-actions>
            </v-card>
          </template>
        </v-dialog>

        <!-- icon button -->
        <v-btn icon='mdi-share' variant='text' @click='snackbar = true' />
      
        <!-- creates snackbar window, similar to dialog -->
        <v-snackbar
          v-model="snackbar"
          location="center">
          This is left intentionally blank to demonstrate the share function.
          
          <!-- slot template -->
          <template v-slot:actions>
            <!-- icon button to close snackbar window -->
            <v-btn icon='mdi-close' @click="onClick" />
          </template>
        </v-snackbar>

        <!-- template -->
        <template v-slot:extension>
          <!-- grid -->
          <v-row>
            <!-- column in grid -->
            <v-col>
              <!-- dynamically creates tabs -->
              <v-tabs
                v-model="tab"
                align-tabs='center'>
                <v-tab v-for='page in pages'>{{ page.tab }}</v-tab>
              </v-tabs>
            </v-col>
          </v-row>
        </template>
      </v-toolbar>

      <!-- window to show pages (content) -->
      <v-window v-model="tab">
        <!-- dynamically creates window items -->
        <v-window-item v-for="item in items"
          :key="item"
          :value="item" >
        </v-window-item>
      </v-window>
    </v-card>

    <!-- gives text -->
    <v-card-text>
      <!-- creates window to show tab content -->
      <v-window v-model='tab'>
        <!-- tabs to the landing (or home) page -->
        <v-window-item>
          <!-- landing vue -->
          <landing />
        </v-window-item>
        <!-- tabs to the shop page -->
        <v-window-item>
          <!-- shop vue -->
          <shop />
        </v-window-item>
        <!-- tabs to the personalize page -->
        <v-window-item>
          <!-- personalize vue -->
          <personalize />
        </v-window-item>
        <!-- tabs to the contact page -->
        <v-window-item>
          <!-- contact vue -->
          <contact />
        </v-window-item>
        <!-- tabs to the discover page -->
        <v-window-item>
          <!-- discover vue -->
          <discover />
        </v-window-item>
        <!-- tabs to the reviews page -->
        <v-window-item>
          <!-- reviews vue -->
          <reviews/>
        </v-window-item>
      </v-window>
    </v-card-text>
  </v-card>
</template>

<script>
  // vue imports //
  import discover from '@/views/Discover.vue'
  import shop from '@/views/Shop.vue';
  import personalize from '@/views/Personalize.vue'
  import contact from '@/views/Contact.vue';
  import reviews from '@/views/Review.vue';
  import landing from '@/views/Landing.vue'
  import cart from '@/views/cart.vue';
  import shoppingCart from '@/views/ShoppingCart.vue'

  export default {
    data: () => ({
      dialog: false,
      snackbar: false,
      loading: false,
      tab: null,
      gold: null,
      // pages object
      pages: [
        {tab: 'Home', href: '@/views/Landing.vue'},
        {tab: 'Shop', href: '@/views/Products.vue'},
        {tab: 'Personalize', href: '@/views/Personalize.vue'},
        {tab: 'Contact', href: '@/views/Contact.vue'},
        {tab: 'Discover', href: '@/views/Discover.vue'},
        {tab: 'Reviews', href: '@/views/Review.vue'},
      ],
      display: {
        mobileBreakpoint: 'sm',
        thresholds: {
          xs: 0,
          sm: 340,
          md: 540,
          lg: 800,
          xl: 1280,
        }
      }
    }),
    watch: {
      loading (val) {
        if (!val) return

        setTimeout(() => (this.loading = false), 2000)
      },
    },
    methods: {
      onClick () {
        this.snackbar = false
      },
    },
    // components
    components: {
      landing,
      shop,
      personalize,
      contact,
      discover,
      reviews,
      cart,
      shoppingCart
    },
  }
</script>
