<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list dense nav>
        <v-list-group
          v-for="(nav_list, i) in nav_lists"
          :key="i"
          :append-icon="nav_list.lists ? undefined : ''"
        >
          <template v-slot:activator>
            <v-list-item :to="nav_list.to">
              <v-list-item-icon>
                <v-icon>{{ nav_list.icon }}</v-icon>
              </v-list-item-icon>
              <v-list-item-content>
                <v-list-item-title>{{ nav_list.name }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </template>
          <v-list-item
            v-for="list in nav_list.lists"
            :key="list.id"
            :to="list.to"
          >
            <v-list-item-icon>
              <v-icon>{{ list.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>{{ list.name }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-group>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? "right" : "left"}` }}</v-icon>
      </v-btn>
      <img
        class="mr-3"
        src="/images/1_2_Basic_Mark_Logo_B.png"
        alt="SBOM Management System"
        height="40"
        style="opacity: 0.7"
      />
      <img
        class="mr-3"
        src="/images/title.png"
        alt="SBOM Management System"
        height="40"
        style="opacity: 0.7"
      />
      <v-spacer />
      <v-btn @click.stop="rightDrawer = !rightDrawer">
        {{ this.username }}
        <v-icon>mdi-account-circle-outline</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer v-model="rightDrawer" :right="right" temporary fixed>
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light> mdi-repeat </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }} HogeHoge Corporation</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      nav_lists: [
        { name: "Home", icon: "mdi-home", to: "/home" },
        {
          name: "I/O",
          icon: "mdi-import",
          to: "",
          lists: [
            { name: "import1", to: "/io/import1" },
            { name: "import2", to: "/io/import2" },
            { name: "import3", to: "/io/import3" },
            { name: "export", to: "/io/export" },
          ],
        },
        {
          name: "Analyze",
          icon: "mdi-chart-bar",
          to: "",
          lists: [
            { name: "search", to: "/analyze/search" },
            { name: "stats", to: "/analyze/stats" },
            { name: "report", to: "/analyze/report" },
          ],
        },
        {
          name: "Manage",
          icon: "mdi-archive-cog",
          to: "",
          lists: [
            { name: "project", to: "/manage/project" },
            { name: "supplier", to: "/manage/supplier" },
            { name: "vulnerability", to: "/manage/vulnerability" },
            { name: "license", to: "/manage/license" },
            { name: "lifecycle", to: "/manage/lifecycle" },
          ],
        },
        {
          name: "System",
          icon: "mdi-cog",
          to: "",
          lists: [
            { name: "user/group/role", to: "/system/user" },
            { name: "scheduler", to: "/system/scheduler" },
            { name: "notice", to: "/system/notice" },
          ],
        },
        { name: "Logout", icon: "mdi-logout", to: "/logout" },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "SBOM Management System",
      username: "unknown:",
    };
  },
  created() {
    this.$nuxt.$on("updateUsername", ($event) => this.updateUsername($event));
  },
  methods: {
    updateUsername(username) {
      this.username = username;
      console.log("updateUsername:" + this.username);
    },
  },
};
</script>
