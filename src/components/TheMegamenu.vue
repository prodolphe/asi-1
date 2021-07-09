<template>
  <nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <a class="navbar-item" href="#">
        <img
          src="./../assets/Logo.png"
        />
      </a>

      <a
        role="button"
        class="navbar-burger"
        aria-label="menu"
        aria-expanded="false"
        data-target="navbarBasicExample"
      >
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
      </a>
    </div>

    <div id="navbarBasicExample" class="navbar-menu">
      <div class="navbar-start">
       
      </div>

      <div class="navbar-end">
        <div class="navbar-item">
          <div class="">
            <a class=""> Comment ça marche ? </a>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script scoped>
import TheSearch from "~/components/TheSearch.vue";

export default {
  components: { TheSearch },
  data() {
    return {
      typeNavbar: "is-primary",
      typesearch: "is-white",
      search: "",
      searchIcon: "search",
      searchbar: "input-desktop-hidden",
      isHideNavbarItem: "",
      logo: "",
      subdomain: "",
    };
  },
  created() {
    this.checkCountry(this.isSubdomain("https://www.benin.studely.com"));
  },
  methods: {
    isSubdomain(url) {
      // IF THERE, REMOVE WHITE SPACE FROM BOTH ENDS
      url = url.replace(new RegExp(/^\s+/), ""); // START
      url = url.replace(new RegExp(/\s+$/), ""); // END

      // IF FOUND, CONVERT BACK SLASHES TO FORWARD SLASHES
      url = url.replace(new RegExp(/\\/g), "/");

      // IF THERE, REMOVES 'http://', 'https://' or 'ftp://' FROM THE START
      url = url.replace(new RegExp(/^http\:\/\/|^https\:\/\/|^ftp\:\/\//i), "");

      // IF THERE, REMOVES 'www.' FROM THE START OF THE STRING
      url = url.replace(new RegExp(/^www\./i), "");

      // REMOVE COMPLETE STRING FROM FIRST FORWARD SLASH ON
      url = url.replace(new RegExp(/\/(.*)/), "");

      // REMOVES '.??.??' OR '.???.??' FROM END - e.g. '.CO.UK', '.COM.AU'
      if (url.match(new RegExp(/\.[a-z]{2,3}\.[a-z]{2}$/i))) {
        url = url.replace(new RegExp(/\.[a-z]{2,3}\.[a-z]{2}$/i), "");

        // REMOVES '.??' or '.???' or '.????' FROM END - e.g. '.US', '.COM', '.INFO'
      } else if (url.match(new RegExp(/\.[a-z]{2,4}$/i))) {
        url = url.replace(new RegExp(/\.[a-z]{2,4}$/i), "");
      }
      return url;
    },

    checkCountry(url) {
      // CHECK TO SEE IF THERE IS A DOT '.' LEFT IN THE STRING

      if (url.match(new RegExp(/\./g)) ? true : false) {
        let subd = url.split(/\.(?=[^\.]+$)/)[0];

        this.subdomain = subd;
      }
    },

    startSearch() {
      this.search === ""
        ? (this.search = "is-fade-out-slow")
        : (this.search = "");

      this.searchIcon === "search"
        ? (this.searchIcon = "times")
        : (this.searchIcon = "search");

      this.searchbar === "input-desktop-hidden"
        ? (this.searchbar = "input-desktop-display")
        : (this.searchbar = "input-desktop-hidden");

      this.isHideNavbarItem === "is-fade-out"
        ? (this.isHideNavbarItem = "is-fade-in")
        : (this.isHideNavbarItem = "is-fade-out");
    },
  },
};
</script>

<style scoped lang="scss">
@import "../variables.scss";

nav {
  height: 6rem;
  background-color: #267EC3;
}

nav a {
  color: #fff;
}
</style>
