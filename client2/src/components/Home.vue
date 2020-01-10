<template>
  <div>
    <v-navigation-drawer
      fixed
      clipped
      app
      v-model="drawer"
    >
      <v-list dense>
        <template v-for="(item, ndx) in menuItems">
          <v-layout
            row
            v-if="item.heading"
            align-center
            :key="ndx"
          >
            <v-flex xs6>
              <v-subheader v-if="item.heading">
                {{ item.heading }}
              </v-subheader>
            </v-flex>
            <v-flex xs6 class="text-xs-center">
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-flex>
          </v-layout>
          <v-list-group v-else-if="item.children" v-model="item.model" v-bind:key="ndx" no-action>
            <v-list-item slot="item" @click="menuAction">
              <v-list-item-action>
                <v-icon>{{ item.model ? item.icon : item['icon-alt'] }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>
                  {{ item.text }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item
              v-for="(child, i) in item.children"
              :key="i"
              @click="menuAction"
            >
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>
                  {{ child.text }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item v-else @click="menuAction" v-bind:key="ndx">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>
                {{ item.text }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      color="blue darken-3"
      dark
      app
      clipped-left
      fixed
    >
      <v-toolbar-title :style="$vuetify.breakpoint.smAndUp ? '' : 'min-width: 72px'" class="ml-0 pl-3">
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <span class="hidden-xs-only">Globomantics Rewards</span>
      </v-toolbar-title>
      <!-- <v-text-field
        light
        solo
        prepend-icon="search"
        placeholder="Search"
        style="max-width: 500px; min-width: 128px"
      ></v-text-field> -->
      <!-- <header-actions></header-actions> -->
    </v-app-bar>
    <v-content>
      <v-container fluid>
        <v-layout>
            <transactions></transactions>
        </v-layout>
      </v-container>
    </v-content>
    <edit-transaction></edit-transaction>
  </div>
</template>

<script>
import Transactions from './Transactions.vue'
import EditTransaction from './EditTransactions.vue'
// import HeaderActions from './HeaderActions.vue'

export default {
  name: 'Home',
  components: {
    Transactions,
    EditTransaction,
   // HeaderActions
  },
  computed: {
    isLoggedIn () {
      return this.$store.getters.isLoggedIn
    }
  },
  data: () => ({
    dialog: false,
    drawer: null,
    menuItems: [
      { icon: 'contacts', text: 'Add Transaction' },
      { icon: 'history', text: 'Current Month' },
      { icon: 'content_copy', text: 'Notes' },
      { icon: 'settings', text: 'Settings' },
      { icon: 'chat_bubble', text: 'Send feedback' },
      { icon: 'help', text: 'Help' }
    ]
  }),
  methods: {
    menuAction: function () {
      // TODO
    },
    showProfile: function () {
      console.log('show profile clicked!')
    }
  },
  mounted: function () {
    console.log('Is user logged in? ', this.isLoggedIn)
    if (!this.isLoggedIn) {
      this.$router.push({ path: '/login' })
    }
  }
}
</script>
