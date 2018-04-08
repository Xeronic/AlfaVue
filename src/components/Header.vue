<template>
<div id="header" class="l-header">
  <div>
    <div class="l-header-wrapper">
      <div class="l-header-inner">
        <div class="l-header-brand">
          <a href="/" class="header-logo-primary">
            <img src="/ui/css/img/logo-alfalaval.png" alt="Alfa Laval">
          </a>
        </div>
        <div class=""></div>
        <div class="l-header-menu-section">
          <div class="l-header-top-menu">
            <TopMenu />
          </div>
          <div class="l-header-bottom-menu">
            <div class="l-header-main-menu">
              <a v-for="menuItem in mainMenu"
                :key="menuItem.Name"
                @click="showMegaMenu(menuItem.ContainerLink)"
                class="c-header-bottom-menu-item"
                >{{ menuItem.Name }}
                <i class="fas fa-angle-down c-header-bottom-menu-indicator"></i>
              </a>
            </div>
            <div class="l-header-bottom-menu-right">
              <a class="c-header-bottom-menu-item">
                {{ contact.Text }}
                <i class="fas fa-angle-down c-header-bottom-menu-indicator"></i>
              </a>
              <a class="c-header-bottom-menu-item c-header-bottom-menu-border-left">
                <i class="fas fa-search padding-right-15"></i>
                {{ search.Name }}
                <i class="fas fa-angle-down c-header-bottom-menu-indicator"></i>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="reverse-header-mobile">
      <div class="c-headerbar">
        <div class="c-headerbar-container">
        </div>
      </div>
      <div v-if="isShowDropDown" class="l-header-dropdown">
        <MegaMenu v-if="selectedMegaMenuCategory" :list="mainMenuSub[selectedMegaMenuCategory]" />
      </div>
    </div>
  </div>
</div>
</template>

<script>
import MegaMenu from "./MegaMenu.vue";
import TopMenu from "./TopMenu.vue";

export default {
  name: "Header",
  components: { TopMenu, MegaMenu },

  data() {
    return {
      contact: {},
      isShowDropDown: false,
      mainMenu: [],
      mainMenuSub: {},
      search: {},
      selectedMegaMenuCategory: null
    };
  },

  created() {
    fetch("https://www.alfalaval.com/webapi/mainmenu/headers/?language=en")
      .then(result => result.json())
      .then(json => {
        this.contact = json.Contact;
        this.mainMenu = json.Mainmenu;
        this.search = json.Search;

        json.Mainmenu.forEach((item, index) => {
          fetch(
            `https://www.alfalaval.com/webapi/menuitems/columns/?index=${index}&language=en`
          )
            .then(result => result.json())
            .then(json => (this.mainMenuSub[item.ContainerLink] = json[0]));
        });
      })
      .catch(error => console.error("Header API: ", error));
  },

  methods: {
    showDropDown() {
      this.isShowDropDown = !this.isShowDropDown;
    },

    showMegaMenu(id) {
      if (id === this.selectedMegaMenuCategory) {
        this.isShowDropDown = false;
        this.selectedMegaMenuCategory = null;
      } else {
        this.selectedMegaMenuCategory = id;
        this.isShowDropDown = true;
      }
    }
  }
};
</script>