<template>
  <div
    class="sidebar-menu-container border-separator"
    id="sidebar-menu-container"
  >
    <IconField
      iconPosition="left"
      :style="{ opacity: visible ? '100%' : '0%' }"
      class="sidebar-menu-input mt-3"
    >
      <InputIcon class="fa-solid fa-bars" style="color: #511c74"> </InputIcon>
      <InputText v-model="menuSlot" placeholder="Slot" />
    </IconField>
    <IconField
      iconPosition="left"
      :style="{ opacity: visible ? '100%' : '0%' }"
      class="sidebar-menu-input my-2 mb-1"
    >
      <InputIcon class="fa-solid fa-magnifying-glass" style="color: #511c74">
      </InputIcon>
      <InputText v-model="menuFilter" placeholder="Procurar" />
    </IconField>
    <div class="sidebar-menu" id="sidebar-menu">
      <div v-if="menuFilter.length > 0">
        <template v-for="(item, i) in menus" :key="i">
          <template v-for="(menuItem, j) in item.pages" :key="j">
            <template v-for="(item2, k) in menuItem.sub" :key="k">
              <template v-for="(item3, k) in item2.sub" :key="k">
                <div
                  v-if="item3.heading && hasFilteredChildren(item3, 3)"
                  class="menu-item"
                >
                  <router-link
                    v-if="item3.route"
                    class="menu-link"
                    active-class="active"
                    :to="item3.route"
                  >
                    <span class="menu-icon">
                      <i
                        :class="item3.menuicon"
                        :title="item3.menutypetooltip"
                        style="color: #511c74"
                        class="fs-4"
                      ></i>
                    </span>
                    <span
                      class="menu-title"
                      style="
                        font-weight: bold;
                        color: #9d9da6;
                        margin-left: 10px;
                      "
                      >{{ item3.heading
                      }}<small
                        ><i
                          class="fas fa-circle-info fa-2xs ms-4"
                          :title="
                            menuItem.sectionTitle + ' - ' + item2.sectionTitle
                          "
                        ></i
                      ></small>
                    </span>
                  </router-link>
                </div>
              </template>
            </template>
          </template>
        </template>
      </div>
      <div v-if="menuFilter.length == 0">
        <template v-for="(item, i) in menus" :key="i">
          <template v-for="(menuItem, j) in item.pages" :key="j">
            <div class="menu-item custom-menu-item">
              <span
                class="menu-link mt-2"
                @click="handleToggleOpen(menuItem, true)"
              >
                <span class="menu-icon" style="margin-left: 4px">
                  <i
                    style="color: #511c74"
                    :class="menuItem.icon"
                    class="fs-4"
                  ></i>
                </span>
                <span v-if="visible" class="menu-title sidebar-menu-text">
                  {{ menuItem.sectionTitle }}
                </span>
                <span
                  class="menu-arrow"
                  style="color: #9d9da6"
                  :class="{
                    'fa-solid fa-caret-down fa-rotate-270': !menuItem.open,
                    'fa-solid fa-caret-down': menuItem.open,
                  }"
                ></span>
              </span>
              <template
                v-if="menuItem.open"
                v-for="(item2, k) in menuItem.sub"
                :key="k"
              >
                <div>
                  <SubMenu
                    v-if="item2 && menuItem.open"
                    :menuItem="menuItem"
                    :menuSection="item2"
                    :lastSub="false"
                    :action="handleToggleOpen"
                    :action2="closeAllItems"
                  ></SubMenu>
                </div>
              </template>
            </div>
          </template>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import SubMenu from "./SubMenu.vue";
import { useRoute } from "vue-router";

export default {
  name: "SideBarMenu",
  components: {
    SubMenu,
  },
  props: ["visible", "action", "menu"],
  data() {
    return {
      menuFilter: "",
      menus: [],
    };
  },
  mounted() {
    this.adaptMenus();
    this.$watch("menu", () => {
      this.adaptMenus();
    });
  },
  watch: {
    visible(value) {
      if (!value) {
        this.closeAllItems(this.menus, true);
      }
    },
  },
  methods: {
    closeAllItems(menuSection, main) {
      if (main) {
        for (let parentMenu of this.menus) {
          for (let sectionMenu of Object.values(parentMenu.pages)) {
            if (sectionMenu.open) {
              console.log("CloseAll If(main)");
              sectionMenu.open = false;
              for (let section of Object.values(sectionMenu.sub)) {
                if (section !== menuSection && section.open) {
                  console.log("CloseAll If(main) subs");
                  section.open = false;
                }
              }
            }
          }
        }
      }
      if (this.menuFilter.length > 0) {
        console.log("CloseAll If(menuLenght)");
        this.menuFilter = "";
      }
    },
    adaptMenus() {
      this.menus = this.menu;
      let newMenus = {};

      for (let menu of this.menus) {
        //Adding a parent
        if (!Object.keys(newMenus).includes(menu.parent)) {
          newMenus[menu.parent] = {
            sectionTitle: menu.parent,
            route: menu.parentroute,
            icon: menu.icon,
            open: false,
            sub: {},
          };
        }
        if (!Object.keys(newMenus[menu.parent].sub).includes(menu.section)) {
          //Adding a section
          let subSectionObj = {
            sectionTitle: menu.section,
            route: menu.sectionroute,
            sub: [],
            open: false,
          };
          newMenus[menu.parent].sub[menu.section] = subSectionObj;
        }

        let subMenuObj = {
          heading: menu.menu,
          route: menu.menuroute,
          menuicon: menu.menuicon,
          menutypetooltip: menu.menutypetooltip,
        };
        newMenus[menu.parent].sub[menu.section].sub.push(subMenuObj);
      }
      this.menus = [{ pages: { ...newMenus } }];
    },
    hasFilteredChildren(menuItem, level) {
      let trigger = false;

      if (level == 1) {
        for (let section of Object.keys(menuItem.sub)) {
          for (let item of menuItem.sub[section].sub) {
            if (
              item.heading
                .normalize("NFD")
                .replace(/\p{Diacritic}/gu, "")
                .toLowerCase()
                .includes(
                  this.menuFilter
                    .normalize("NFD")
                    .replace(/\p{Diacritic}/gu, "")
                    .toLowerCase()
                )
            ) {
              trigger = true;
            }
          }
        }
      } else if (level == 2) {
        for (let item of menuItem.sub) {
          if (
            item.heading
              .normalize("NFD")
              .replace(/\p{Diacritic}/gu, "")
              .toLowerCase()
              .includes(
                this.menuFilter
                  .normalize("NFD")
                  .replace(/\p{Diacritic}/gu, "")
                  .toLowerCase()
              )
          ) {
            trigger = true;
          }
        }
      } else if (level == 3) {
        if (
          menuItem.heading
            .normalize("NFD")
            .replace(/\p{Diacritic}/gu, "")
            .toLowerCase()
            .includes(
              this.menuFilter
                .normalize("NFD")
                .replace(/\p{Diacritic}/gu, "")
                .toLowerCase()
            )
        ) {
          trigger = true;
        }
      }
      return trigger;
    },
    handleToggleOpen(menuSection, main) {
      //Closing when changing on the principal selection
      if (main) {
        for (let parentMenu of this.menus) {
          for (let sectionMenu of Object.values(parentMenu.pages)) {
            if (sectionMenu !== menuSection && sectionMenu.open) {
              console.log("ToogleOpen If(main)");
              sectionMenu.open = false;
              for (let section of Object.values(sectionMenu.sub)) {
                if (section !== menuSection && section.open) {
                  console.log("ToogleOpen If(main) subs");
                  section.open = false;
                }
              }
            }
          }
        }
      } else {
        for (let parentMenu of this.menus) {
          for (let sectionMenu of Object.values(parentMenu.pages)) {
            for (let section of Object.values(sectionMenu.sub)) {
              if (section !== menuSection && section.open) {
                console.log("ToogleOpen else(!main)");
                section.open = false;
              }
            }
          }
        }
      }
      console.log(menuSection);
      console.log("ToogleOpen ok");
      //Closing or Opening the section we want
      if (menuSection.open) menuSection.open = false;
      else menuSection.open = true;
      //If the tab is closed, open it so it shows the section
      if (!this.visible) {
        console.log("ToogleOpen If(!visible)");
        this.action();
      }
    },
  },
  setup() {
    const route = useRoute();

    const hasActiveChildren = (match) => {
      return route.path.indexOf(match) !== -1;
    };

    return { hasActiveChildren };
  },
};
</script>
./SubMenu.vue
