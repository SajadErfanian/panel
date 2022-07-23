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
            <v-list-item
              v-for="item in actualMenu"
              :key="item.id"
              :to="item.url"
            >
              <v-list-item-icon>
                <v-icon>mdi-home</v-icon>
              </v-list-item-icon>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
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

                <v-list-item
                  v-for="item in subMenu"
                  :key="item.idi"
                  link
                  :to="item.url"
                >
                  <v-list-item-title v-text="item.title"></v-list-item-title>

                  <!-- <v-list-item-icon>
                    <v-icon v-text="icon"></v-icon>
                  </v-list-item-icon> -->
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
import axios from "axios";
export default {
  name: "SideBar",
  data() {
    return {
      clipped: false,
      drawer: true,
      actualMenu: [],
      subMenu: [],
      fixed: true,
      mini: false,
      offset: true,
      property: "Blank",
      on: null,
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
      token:
        "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI5YWNiM2E4Yy0zMGE3LTRlMGQtODgzNS1iYzc2YmU1OTQ3ZGMiLCJpc3MiOiJodHRwOi8vbGdjMi13ZWJhcGktc3RnOjM0MTAvIiwiaWF0IjoxNjU1ODQxODk2LCJEZXZpY2VJZCI6IlNGTWZ2V2d4VnQ5VG8xVmE3VUMvSkZQNzAwdmNXNEFLQkJIczkyOUZicXA4SUwvYlJuWFdhVTlOWTNjSXNhbEJLZ0tJUWR2MTcvQzlXdjB2Nm9qSDlWK3ZheGpsNGpTQVg2YWI5ZCtKL0hVUHdSbTRjTndmMk9JYkpYeGplSVNQSWlMS3ZKaFRyQzZRMEJsclRaWEp2UzVLY0pjdkFnWnp2YmQ1WEVvVXlYLzk1dDMva3NBYzRqWlZaNVNrWWxUMUZxYi9NcGlOYnI1cHVwaGhBZG04SVhpbkxWcjdkSVFwditYYkxZV1hCZFdMV0VYTFMvZmxWTjJ5WlBybGc5VzBZSGN6enlOMXU0aGdheW5sM3EvR1R2MjNPNUFzU3VxTTlGalFSYTZ6MVJmbmFMbHpYbWxnZWgvcjh2bXZmaDJ4V0ZUL0MwY0kzdndTcGRtOXlLRTVQbkVSYUhDdDVLRkZrUW9WcnhDUzMrdVQ4ZkE2WlZERURMWWY3TTNzcGhOYW9obS85SDZNdTRJYk9NamppOEd5WTB1QUZlUGdna1RaTGNQZ0VlemhwSWdXK293Y1F5bjVFNlg1Q1FxVU5WL1EwOUJDSHlwcmVMODhXVWF2b0tjNUZTN2ZlSmZvS2lJaW1vZlVBcnVTdmJKZ2hJaTNxQ2tweS94MEt3WHpBTU9RZGd4N2RTZGFYN1JXR3MyYWxkenpYWEc1NHZEeWlOdXZDV2dkbWNRcmQ2ZXFibkFlbk5TcEdya1FnWnpXOFo5OVdIUTFRL2ZpZUVHdEJRQlNJNCtiUGRMYU5HRHBqbGc3SktnSlFja0xlM0M5UGFqRHZ5R0k4dmd6ckQ0d0dwQ2Z1emZGM1ZrVUU3SlNGak1tdE02Vy84RUdvUkZjRExkdlhIR3BRaDExM1BVPSIsImh0dHA6Ly9zY2hlbWFzLnhtbHNvYXAub3JnL3dzLzIwMDUvMDUvaWRlbnRpdHkvY2xhaW1zL21vYmlsZXBob25lIjoiQjZWWXlBV3RUS1BydTJRVmpTM0ZCcEdPVmpjR2lHRWMycTF2VExwTE9JTWxXOFFqV2tUVSszNFBhMTZhQ1JvcU5BY1RvRjNSZmJTa0RZWjJKbURLWG5jTWhJSHcybVlqR1RiRGVPd2k1RkN6am1MUktzdGJ0WkFFR1RsOXVlR3l1YUJ4RDBuM3pDQUpyV0kyeTRIL0NpMzVTQXFadVZSdHpRdHMxbXpyblVMK0t2aVEyVmt3ckJ4Wkd1Q05na2RCVGFYOXpaakE1RkZMM29FTmpaOWlOU2ZlY3RneUxKQzVmL3JlTG82UFJOeUZFZjM1dE9CVThvY0lUb0tQSWFTWHNNT2hPTEpYMHBlQnNmai9UclFTT0s2RDJEOFVNc050Y2orRUJkM3Y5VnlWNWVXdWNtQ3JRZW1SUHhIUXdHWVlPc3ZYR2w1ZmpzNngyMHNvSWxLU21JR0pKaUVFWmtUSGNCajNmQmhSNVlnNmUya3c4eUxObmkvUjJMSFRZV3VrZE1PQUxNWnVLWWFUMy9zOEYvWXJqNDROL0RNQmJrSU1nQ2RuYVlvTjVJYkw1YnNGUjdFNURaaEN3T1MrUHJDdDZRN1hmYno3SGNFd1hRQ1dwMUtaNjZSTWJ5UHkyL2N5bVF3MXJva1FvM0lCT0xzZUVXRVFDblpqRUdJY2JVbFdKNTZ2UFhPYVB6cGFsSmFZWi9tYndHV2JvL04vdWp3UkxQQzJxaERaQlpFdjZ3cVpuenNkU1IwMXBiT0hGdk9rUnVjUEZBR2traWdjclZMN3hyaU9BemUrUXRnd3djcmlVdXdJZTAyV2VySFJENWhFOFNjajlTRmtiQTdTVHlpbmI1KzZnTUlta3BLWUN5OSt6VXBNSVNoZVo2Snk5MnVCclJhM0JsSldLUlE9IiwiVXNlciI6IjIiLCJBcHBsaWNhdGlvbklkIjoiUGFuZWwiLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOiJNZEc2MkNPNnFxMDE5USttSTRXNk1VeGZDdEl3R1ZScngyVXdEak1pdjJKeTZ3S0tpVXJlWE56V1hHNDBJTkJ3RnV5QXhOcmNYbE5yM053RUhKdFQ3Rm4yV1lhQ3RJd0lpSlhRRWsyeUVXSnlUbERZU21SeWJyaWluQW9iNzdUSHc2MTBLckVlSnFBUEc4MnRJZ2x0bXFwVkY2VG96NnUvTjFldS8wNjdSdVpTNlc0djM2MG5qNkR1dmR6VitiaEhQR0tQTC9VWjNJWXlLNE8rVlRyQmp1RWhhRXN4NnRub2xnakxXTVhSSHNJbi8xaTc1TjJwUHNiK3h2RDRyd0YwUWFEWUFHNzdDMVVZUWkvU29vMVVyYXdsczkzSjJ6VGtvQ3ZvTEk4d1FpU1grNUQyclBlRGdmUG1MRENQeWx2WkxqOFlxbm1kWk91MDVLK2RoUUJpT1FLdDdNazhqMXdRRWVBWjJSUGx0SUhVZndwRkh1ZkcvTTBuZHVtajhpc0NoeThkZktCRnZDWm8yMmpScThvS2hoTlUrUVJEMWxnNmlQY1ZTQW1mMFRDRlRnZWp0N2pmU2hNdWZDeVFtY2FpODdoZ2pMMjJLWURCaXR0MFhqNytyS2lLMHcxR1BMSEFCQ09yT0pXMHIxTDVzYWQzMDVDaTdxNjNVTFVpejhZZjd4N0w4RGpPNFJ4bE9kT28xN09scHE4RlFRT0krbHNpZGVmSHhLa0R0alN3SEVPRFBpTldsQ2YwSFVDWWxJcXBLMXdoOHNoYjYyL2EyUkFpRUFnQnd0VUtCbTVPVjdvOFZqUnZVZGtMMjJOdng1TXRvbm41SWRKNHdtY3haL0tlV0JrZFlGRFV1MnpiSUZSRU9tbVBuaVVlMGVaWTZJN3EreEJaYjIzRHc0TT0iLCJSb2xlSWQiOiJvd25lcl9jb21wYW55IiwiQ29tcGFueSI6IjEiLCJuYmYiOjE2NTU4NDE4OTgsImV4cCI6MTY2MTg0MTg5OCwiYXVkIjoiQW55In0.Bfc1j4MbrjFSOYYtKXuXIJdb345OBSf4JxgTlD87BWiQKr56POVKHdeUOdfPMaN1pouD25JSLPl_d4jCQnxuiQ",
    };
  },
  computed: {},
  created() {
    this.updateMenu();
  },
  mounted() {},
  methods: {
    updateMenu() {
      axios
        .get("https://auth.fmj.ir/api/role/permissions", {
          headers: {
            Authorization: `Bearer ${this.token}`,
            DeviceId: "test-front",
          },
        })
        .then((res) => {
          this.actualMenu = res.data.content.filter(function (item) {
            return item.items === null;
          });
          console.log("actual menu", this.actualMenu);
          this.subMenu = res.data.content.filter(function (item) {
            return item.items != null;
          });
          console.log("sub menu", this.subMenu[0]);
        })
        .catch((err) => console.log(err));
    },
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




