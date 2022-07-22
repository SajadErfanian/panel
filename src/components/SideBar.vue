<template>
  <v-card>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant.sync="mini"
      permanent
      :clipped="clipped"
      fixed
      app
    >
      <!-- top section -->
      <div>
        <!-- logo -->
        <v-list-item class="px-2">
          <v-list-item-avatar>
            <v-img src="../assets/logo.png"></v-img>
          </v-list-item-avatar>

          <v-list-item-title>John Leider</v-list-item-title>

          <v-btn icon @click.stop="mini = !mini">
            <v-icon>mdi-chevron-left</v-icon>
          </v-btn>
        </v-list-item>
        <!-- logo -->
        <v-divider></v-divider>
        <!-- sid-bar items -->
        <v-card class="mx-auto" width="300">
          <v-list>
            <v-list-item>
              <v-list-item-icon>
                <v-icon>mdi-home</v-icon>
              </v-list-item-icon>

              <v-list-item-title>Home</v-list-item-title>
            </v-list-item>

            <v-list-group prepend-icon="mdi-account-circle">
              <template v-slot:activator>
                <v-list-item-title>Users</v-list-item-title>
              </template>
              <v-list-group no-action sub-group>
                <template v-slot:activator>
                  <v-list-item-content>
                    <v-list-item-title>Actions</v-list-item-title>
                  </v-list-item-content>
                </template>

                <v-list-item v-for="([title, icon], i) in cruds" :key="i" link>
                  <v-list-item-title v-text="title"></v-list-item-title>

                  <v-list-item-icon>
                    <v-icon v-text="icon"></v-icon>
                  </v-list-item-icon>
                </v-list-item>
              </v-list-group>
            </v-list-group>
          </v-list>
        </v-card>
        <!-- sid-bar items -->
      </div>
      <!-- top section -->
      <!-- profile menu -->
      <div>
        <v-menu top :offset-y="offset" min-width="150">
          <template v-slot:activator="{ on, attrs }">
            <v-list-item class="px-2" v-bind="attrs" v-on="on">
              <v-list-item-avatar>
                <v-img src="../assets/user.png"></v-img>
              </v-list-item-avatar>
              <v-list-item-title>John Leider </v-list-item-title>
              <span>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  role="img"
                  aria-hidden="true"
                  class="v-icon__svg"
                >
                  <path d="M7,10L12,15L17,10H7Z"></path></svg
              ></span>
            </v-list-item>
          </template>
          <v-list>
            <v-list-item v-for="(item, index) in userProfile" :key="index">
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </div>
      <!-- profile menu -->
    </v-navigation-drawer>
  </v-card>
</template>

<script>
export default {
  name: "SideBar",
  data() {
    return {
      clipped: false,
      drawer: true,
      fixed: true,
      mini: false,
      offset: true,
      on: null,
      items: [
        {
          icon: "mdi-home-outline",
          title: "Home",
          to: "/",
        },
        {
          icon: "mdi-speedometer",
          title: "Dashboard",
          to: "/Dashboard",
        },
        {
          icon: "mdi-table",
          title: "Orders",
          to: "/Orders",
        },
        {
          icon: "mdi-apps",
          title: "Products",
          to: "/Products",
        },
        {
          icon: "mdi-account-outline",
          title: "Customers",
          to: "/Customers",
        },
      ],
      userProfile: [
        { title: "New project" },
        { title: "Settings" },
        { title: "Profile" },
        { title: "Sign out" },
      ],
      admins: [
        ["Management", "mdi-account-multiple-outline"],
        ["Settings", "mdi-cog-outline"],
      ],
      cruds: [
        ["Create", "mdi-plus-outline"],
        ["Read", "mdi-file-outline"],
        ["Update", "mdi-update"],
        ["Delete", "mdi-delete"],
      ],
    };
  },
};
</script>
<style scoped>
::v-deep .v-navigation-drawer__content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.v-icon__svg {
  vertical-align: middle;
}
</style>
