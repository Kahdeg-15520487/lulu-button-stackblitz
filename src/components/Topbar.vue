<template>
  <v-app-bar
    :collapse-on-scroll="$vuetify.breakpoint.smAndDown"
    fixed
    :color="night"
    dark
    :elevate-on-scroll="$vuetify.breakpoint.mdAndUp"
  >
    <v-menu transition="slide-y-transition" bottom offset-y>
      <template v-slot:activator="{on:menu}">
        <v-app-bar-nav-icon v-on="menu" v-show="$vuetify.breakpoint.smAndDown"></v-app-bar-nav-icon>
      </template>
      <v-list>
        <v-list-item @click="see('https://www.youtube.com/channel/UC_a1ZYZ8ZTXpjg9xUY9sj8w')">
          <v-list-item-title>
            <v-icon left color="red">mdi-youtube</v-icon>鈴原るる【にじさんじ所属】
          </v-list-item-title>
        </v-list-item>
        <v-list-item @click="see('https://twitter.com/lulu_suzuhara')">
          <v-list-item-title>
            <v-icon left color="blue">mdi-twitter</v-icon>@lulu_suzuhara
          </v-list-item-title>
        </v-list-item>
        <v-divider></v-divider>
        <v-list-item @click="see('https://github.com/RomainLK/lulu-button')">
          <v-list-item-title>
            <v-icon left color="black">mdi-github</v-icon>
            {{$t("ui.helpdevelope")}}
          </v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
    <v-toolbar-title>{{$t("ui.title")}}</v-toolbar-title>
    <v-btn
      v-if="$vuetify.breakpoint.mdAndUp"
      class="ml-4 text-capitalize"
      color="red"
      @click="see('https://www.youtube.com/channel/UC_a1ZYZ8ZTXpjg9xUY9sj8w')"
    >
      <v-icon left>mdi-youtube</v-icon>鈴原るる【にじさんじ所属】
    </v-btn>
    <v-btn
      v-if="$vuetify.breakpoint.mdAndUp"
      class="ml-4 text-lowercase"
      color="blue"
      @click="see('https://twitter.com/lulu_suzuhara')"
    >
      <v-icon left>mdi-twitter</v-icon>@lulu_suzuhara
    </v-btn>
    <v-spacer></v-spacer>
    <v-btn
      v-if="$vuetify.breakpoint.mdAndUp"
      @click="see('https://github.com/RomainLK/lulu-button')"
    >
      <v-icon left>mdi-github</v-icon>
      {{$t("ui.helpdevelope")}}
    </v-btn>
    
    <v-btn
      v-if="$vuetify.breakpoint.mdAndUp"
      @click="see('https://github.com/RomainLK/lulu-button')"
    >
      <v-icon left></v-icon>
      {{$t("ui.helpdevelope")}}
    </v-btn>
    <v-btn :disabled="!$store.state.lastAudio" fab icon @click="copyLastAudio()">
      <v-icon>mdi-content-copy</v-icon>
    </v-btn>
    <v-btn fab icon @click="darkMode()">
      <v-icon>{{darkmodeicon}}</v-icon>
    </v-btn>
    <v-menu transition="slide-y-transition" bottom offset-y>
      <template v-slot:activator="{on:menu}">
        <v-fab-transition>
          <v-btn v-on="menu" fab icon>
            <v-icon>mdi-earth</v-icon>
          </v-btn>
        </v-fab-transition>
      </template>
      <v-list>
        <v-list-item
          v-for="(lang, i) in langs"
          :key="i"
          :disabled="!lang.isready"
          @click="changeLang(lang.src)"
        >
          <v-list-item-title>{{ lang.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
    <v-snackbar color="success" v-model="snackbarCopy">
      <span>{{$t('ui.copiedToClipboard', {value: $store.state.lastAudio})}}</span>
      <template v-slot:action="{attrs}">
        <v-btn color="pink" text v-bind="attrs" @click="snackbarCopy = false">Close</v-btn>
      </template>
    </v-snackbar>
  </v-app-bar>
</template>

<script>
export default {
  data: () => ({
    snackbarCopy: false,
    langs: [
      { title: "English", src: "en", isready: true },
      { title: "简体中文", src: "zhHans", isready: false },
      { title: "日本語(ベータ版)", src: "ja", isready: false },
    ],
  }),
  methods: {
    changeLang(e) {
      this.$i18n.locale = e;
      this.$cookies.set("Lang", e);
    },
    darkMode() {
      this.$store.commit("change_dark_mode");
      this.$vuetify.theme.dark = this.$store.state.dark_mode;
    },
    see(e) {
      window.location.href = e;
    },
    copyLastAudio() {
      this.copyToClipboard(this.$store.state.lastAudio);
      this.snackbarCopy = true;
    },
    copyToClipboard(text) {
      const listener = (ev) => {
        ev.preventDefault();
        ev.clipboardData.setData("text/plain", text);
      };
      document.addEventListener("copy", listener);
      document.execCommand("copy");
      document.removeEventListener("copy", listener);
    },
  },
  computed: {
    darkmodeicon: {
      get: function () {
        return this.$store.state.dark_icon;
      },
    },
    night: {
      get: function () {
        if (this.$vuetify.theme.dark) {
          return "grey darken-4";
        } else {
          return "primary";
        }
      },
    },
  },
};
</script>

<style>
</style>