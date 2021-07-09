<template>
  <Layout>
    <!-- Learn how to use images here: https://gridsome.org/docs/images -->
    <section class="section is-large bg-img mb-7">
      <div class="subtitle">
        <h1 class="title has-text-primary-light is-uppercase has-text-weight-light">Bienvenue sur</h1>
        <p class="title is-2">
          <strong class="has-text-primary-light"
            >La plateforme des potentialités d’Afrique</strong
          >
        </p>
      </div>
      <div>
        <div class=" field has-addons has-addons-centered ">
          <p class="control">
            <span class="select is-large">
              <select>
                <option>Fintech</option>
                <option>Agronomie</option>
                <option>Elevage</option>
              </select>
            </span>
          </p>
          <p class="control">
            <input class="input is-large" type="text" placeholder="Recherche" />
          </p>
          <p class="control">
            <a class="button search-btn is-large">
              <i class="fas fa-search"></i>
            </a>
          </p>
        </div>
        <div>
        <p class="subtitle is-5 has-text-grey-lighter">
          Retrouvez les trois meilleurs résultats de votre recherche
        </p>
      </div>
      </div>
      
    </section>
  </Layout>
</template>

<page-query>
query Page($path: String! = "/index") {
  page: contentfulPage(path: $path) {
    id
    path
    title
    coverVideo {
      file {
        url
      }
    }
    carrouselBlock {
      indicator
      arrow
      arrowBoth
      arrowHover
      autoPlay
      pauseHover
      carrouselBlock {
        title
        image {
          file {
            url
          }
        }
        description
        button {
          title
          link
        }
        country
      }
    }
    services {
      title
      description
      coverImage {
        file {
          url
        }
      }
      button {
        title
        link
      }
      pictoBefore {
        file {
          url
        }
      }
      pictoAfter {
        file {
          url
        }
      }
    }
    testimonialsSectionTitle
    testimonials {
      title
      date
      url
      description
    }
    aboutSectionTitle
    aboutSection {
      title
      aboutSection {
        title
        image {
          title
          file {
            url
          }
          description
        }
        subtitle
        description
        button {
          title
          link
        }
      }
      team {
        title
        teamOne {
          file {
            url
          }
        }
        teamTwo {
          file {
            url
          }
        }
        teamThree {
          file {
            url
          }
        }
        teamFour {
          file {
            url
          }
        }
      }
      events {
        title
        events(sortBy: "date", order: ASC) {
          id
          title
          country
          description
          date
          startTime
          endTime
          image {
            file {
              url
            }
          }
        }
      }
      country
    }
    statsSectionTitle
    stats {
      title
      image {
        file {
          url
        }
      }
      description
    }
    partnersSectionTitle
    partners {
      images {
        file {
          url
        }
      }
    }
    feedsSectionTitle
    feeds {
      title
      image {
        file {
          url
        }
      }
      source
      description
    }
  }
  countries: allContentfulListeFiliales {
    edges {
      node {
        countries
      }
    }
  }
}
</page-query>

<script>
import services from "~/components/services.vue";
import Partner from "~/components/Partner.vue";
import Apropos from "~/components/Apropos.vue";

export default {
  components: {
    services,
    Partner,
    Apropos,
  },
  metaInfo: {
    title: "Studely : Etudier en France en toute sérénité !",
  },
  data() {
    return {
      isImageModalActive: false,
      isCardModalActive: false,
      iconPack: "fas",
      iconSize: "is-large",
      pauseHover: false,
      carousels: [],

      //Main carrousel

      arrow: false,
      arrowBoth: false,
      arrowHover: false,
      iconPack: "mdi",
      iconPrev: "arrow-left",
      iconNext: "arrow-right",
      iconSize: "",
      indicator: false,
      autoPlay: false,
      pauseHover: false,

      carousel: [],

      about: [],
      events: [],

      page: {},
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
        let subdomain = url.split(/\.(?=[^\.]+$)/)[0];

        let countries = this.$page.countries.edges[0].node.countries;

        let country = countries.filter((item) => item === subdomain);

        if (country.length > 0) {
          this.loadCountryData(subdomain);
        }
      }
    },

    loadCountryData(subdomain) {
      //filter page by domain

      let page = [this.$page.page];
      this.page = page[0];

      this.arrow = this.page.carrouselBlock.arrow;
      this.arrowBoth = this.page.carrouselBlock.arrowBoth;
      this.arrowHover = this.page.carrouselBlock.arrowHover;
      this.indicator = this.page.carrouselBlock.indicator;
      this.autoPlay = this.page.carrouselBlock.autoPlay;
      this.pauseHover = this.page.carrouselBlock.pauseHover;

      //filter carousel by domain

      let carousel = this.page.carrouselBlock.carrouselBlock;
      this.carousel = carousel.filter((item) => item.country === subdomain);

      this.carousels = this.page.feeds;

      //filter about by domain

      let about = this.page.aboutSection;
      this.about = about.filter((item) => item.country === subdomain);

      this.events = about[0].events.events.filter(
        (item) => item.country === subdomain
      );

      console.log(this.page);
    },
  },
  filters: {
    // Filter definitions
    Upper(value) {
      const leftletter = value.substring(0, 1);
      const rightvalue = value.substring(1);
      return leftletter.toUpperCase() + rightvalue;
    },
  },
};
</script>

<style lang="scss">
.search-btn {
  background-color: #ff9b26;
  color: #fff;
}

.bg-img {
  background-image: url("./../assets/bg.png");
  background-position: center center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: cover;
  background-color: #FBFDFF;
}

white-text {
  color: #fff !important;
}
/*
.is-spaced {
  font-size: 20px;
  font-weight: 600;
  text-align: center;
}

.home-links a {
  margin-right: 1rem;
}

.diapo {
  min-height: 40em;
  background-size: cover;
}

.content-24 {
  font-size: 2em;
}

@media only screen and (max-width: 414px) {
  .content-24 {
    font-size: 1.5em;
  }

  .button-service {
    font-size: 1em !important;
  }
}

.image-fit {
  width: 80px;
  height: 80px;
}*/
</style>
