<template>
  <div
    class="flex-column-auto pt-2 pb-6 sidebar-options-background-color"
    ref="sidebar"
  >
    <div
      v-if="visible"
      class="d-flex align-items-center sidebar-options-container"
    >
      <div>
        <i
          :class="iconNotifications + ' sidebar-options-btn-icon-active-color'"
          @click="toggleCardVisibilityNotifications($event)"
        ></i>
      </div>

      <Card
        v-if="cardVisibleNotifications"
        :style="{
          top: cardPositionNotifications.top + 'px',
          right: cardPositionNotifications.right + 'px',
          position: 'absolute',
          boxShadow: '0 5px 10px rgba(0, 0, 0, 0.5)',
          height: '300px',
          width: '400px',
          overflow: 'hidden',       
    }"
      >
        <template #title>
          <div class="notif-search">
            <div
              style="
                display: flex;
                justify-content: space-between;
                align-items: baseline;
              "
            >
              <div>Notificações</div>
              <div
                class="sidebar-option-icons-component"
                style="display: flex; align-items: baseline"
              >
                <span style="font-size: small">
                  Marcar todas como lidas
                  <i
                    class="fa-solid fa-check-double fs-6"
                    style="margin-left: 5px"
                  ></i>
                </span>
              </div>
            </div>
            <IconField
              iconPosition="left"
              :style="{ opacity: visible ? '100%' : '0%' }"
            >
              <InputIcon
                class="fa-solid fa-magnifying-glass fs-6"
                style="color: #511c74; margin-left: 8px"
              ></InputIcon>
              <InputText
                v-model="menuSlot"
                placeholder="Procurar"
                class="sidebar-options-input"
              />
            </IconField>
          </div>
        </template>

        <template #content>
          <div class="sidebar-options-notification-container">
            <p class="m-0" v-if="notifications.length === 0">
              Sem Notificações
            </p>
            <div
              v-if="notifications.length > 0"
              class="sidebar-options-notification-scroll"
            >
              <template v-for="(notif, i) in notifications" :key="i">
                <div class="sidebar-options-notification-item">
                  <div class="sidebar-options-flex-container">
                    <i
                      class="fa-solid fa-bell fs-5 sidebar-options-circle-icon"
                    ></i>
                    <div class="sidebar-options-notification-content">
                      <h6>{{ notif.title }}</h6>
                      <span>{{ notif.description }}</span>
                      <br />
                      <span>{{ notif.date }}</span>
                    </div>
                    <i
                      v-tooltip.top="'Eliminar'"
                      class="fa-solid fa-xmark sidebar-options-close-icon"
                    ></i>
                  </div>
                </div>
              </template>
            </div>
          </div>
        </template>
      </Card>

      <div>
        <i
          :class="iconLastInstances + ' sidebar-options-btn-icon-active-color'"
          @click="toggleCardVisibilityLastInstances($event)"
        ></i>
      </div>

      <Card
        v-if="cardVisibleLastInstances"
        :style="{
          top: cardPositionLastInstances.top + 'px',
          right: cardPositionLastInstances.right + 'px',
          position: 'absolute',
          boxShadow: '0 5px 10px rgba(0, 0, 0, 0.5)',
          height: '300px',
          width: '400px',
        }"
      >
        <template #title>
          <div class="notif-search">
            <div
              style="
                display: flex;
                justify-content: space-between;
                align-items: baseline;
              "
            >
              <div>Últimas Instâncias</div>
              <div
                class="sidebar-option-icons-component"
                style="display: flex; align-items: baseline"
              >
                <i
                  v-tooltip.top="'Favoritar'"
                  class="fa-solid fa-star fs-6"
                  style="margin-left: 10px; font-size: small"
                ></i>
                <i
                  v-tooltip.top="'Filtrar A-Z'"
                  class="fa-solid fa-arrow-up-z-a fs-6"
                  style="margin-left: 10px; font-size: small"
                ></i>
                <i
                  v-tooltip.top="'Filtrar Z-A'"
                  class="fa-solid fa-arrow-down-z-a fs-6"
                  style="margin-left: 10px; font-size: small"
                ></i>
              </div>
            </div>
            <IconField
              iconPosition="left"
              :style="{ opacity: visible ? '100%' : '0%' }"
            >
              <InputIcon
                class="fa-solid fa-magnifying-glass fs-6"
                style="color: #511c74; margin-left: 8px"
              ></InputIcon>
              <InputText
                v-model="menuSlot"
                placeholder="Procurar"
                class="sidebar-options-input"
              />
            </IconField>
          </div>
        </template>
        <template #content>
          <div class="sidebar-options-notification-container">
            <p class="m-0" v-if="lastInstances.length === 0">
              Sem Últimas Instâncias
            </p>
            <div
              v-if="lastInstances.length > 0"
              class="sidebar-options-notification-scroll"
            >
              <template v-for="(instances, i) in lastInstances" :key="i">
                <div class="sidebar-options-notification-item">
                  <div class="sidebar-options-flex-container">
                    <i
                      class="fa-solid fa-star fs-5 sidebar-options-circle-icon"
                    ></i>
                    <div class="sidebar-options-notification-content">
                      <h6>{{ instances.context }}</h6>
                      <span>{{ instances.url }}</span>
                      <span>{{ instances.instanceid }}</span>
                      <span>{{ instances.ts }}</span>
                      <span>{{ instances.routerpath }}</span>
                      <span>{{ instances.favbit }}</span>
                      <span>{{ instances.menutype }}</span>
                      <span>{{ instances.entryi }}</span>
                      <br />
                    </div>
                  </div>
                </div>
              </template>
            </div>
          </div>
        </template>
      </Card>

      <div>
        <i
          :class="iconDocs + ' sidebar-options-btn-icon-active-color'"
          @click="toggleCardVisibilityDocs($event)"
        ></i>
      </div>

      <Card
        v-if="cardVisibleDocs"
        :style="{
          top: cardPositionDocs.top + 'px',
          right: cardPositionDocs.right + 'px',
          position: 'absolute',
          boxShadow: '0 5px 10px rgba(0, 0, 0, 0.5)',
          height: '300px',
          width: '400px',
        }"
      >
        <template #title>
          <div class="notif-search">
            <div
              style="
                display: flex;
                justify-content: space-between;
                align-items: baseline;
              "
            >
              <div>Documentos</div>
              <div
                class="sidebar-option-icons-component"
                style="display: flex; align-items: baseline"
              >
                <i
                  v-tooltip.top="'Download Todos'"
                  class="fa-solid fa-download fs-6"
                  style="margin-left: 10px; font-size: small"
                ></i>
                <i
                  v-tooltip.top="'Apagar Todos'"
                  class="fa-solid fa-trash fs-6"
                  style="margin-left: 10px; font-size: small"
                ></i>
                <i
                  v-tooltip.top="'Filtrar A-Z'"
                  class="fa-solid fa-arrow-up-z-a fs-6"
                  style="margin-left: 10px; font-size: small"
                ></i>
                <i
                  v-tooltip.top="'Filtrar Z-A'"
                  class="fa-solid fa-arrow-down-z-a fs-6"
                  style="margin-left: 10px; font-size: small"
                ></i>
              </div>
            </div>
            <IconField
              iconPosition="left"
              :style="{ opacity: visible ? '100%' : '0%' }"
            >
              <InputIcon
                class="fa-solid fa-magnifying-glass fs-6"
                style="color: #511c74; margin-left: 8px"
              ></InputIcon>
              <InputText
                v-model="menuSlot"
                placeholder="Procurar"
                class="sidebar-options-input"
              />
            </IconField>
          </div>
        </template>
        <template #content>
          <div class="sidebar-options-notification-container">
            <p class="m-0" v-if="documents.length === 0">
              Sem Últimas Instâncias
            </p>
            <div
              v-if="documents.length > 0"
              class="sidebar-options-notification-scroll"
            >
              <template v-for="(docs, i) in documents" :key="i">
                <div class="sidebar-options-notification-item">
                  <div class="sidebar-options-flex-container">
                    <i
                      class="fa-solid fa-file-pdf fs-5 sidebar-options-circle-icon"
                    ></i>
                    <div class="sidebar-options-notification-content">
                      <h6>{{ docs.reportname }}</h6>
                      <span>{{ docs.format }}</span>
                      <span>{{ docs.generationts }}</span>
                      <br />
                    </div>
                    <div class="sidebar-options-close-icon">
                      <i
                        v-tooltip.top="'Download'"
                        class="fa-solid fa-file-download"
                        style="margin-right: 10px"
                      ></i>
                      <i
                        v-tooltip.top="'Eliminar'"
                        class="fa-solid fa-xmark"
                      ></i>
                    </div>
                  </div>
                </div>
              </template>
            </div>
          </div>
        </template>
      </Card>

      <div>
        <i
          :class="iconTheme + ' sidebar-options-btn-icon-active-color'"
          @click="toggleIconTheme($event)"
        ></i>
      </div>
    </div>

    <div v-else class="d-flex align-items-center justify-content-center p-2">
      <i class="fa-solid fa-gears fs-2 sidebar-options-color-icon-options"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: "SideBarOptions",
  props: ["visible", "notification", "lastInstancesProp", "documentsProp"],
  data() {
    return {
      cardVisibleLastInstances: false,
      cardVisibleNotifications: false,
      cardVisibleDocs: false,
      cardPositionLastInstances: { top: 0, right: 0 },
      cardPositionNotifications: { top: 0, right: 0 },
      iconNotifications: "fa-solid fa-bell fs-4",
      iconLastInstances: "fa-solid fa-clock-rotate-left fs-4",
      iconDocs: "fa-solid fa-file-lines fs-4",
      iconTheme: "fa-solid fa-sun fs-4",
      notifications: [],
      lastInstances: [],
      documents: [],
    };
  },
  methods: {
    toggleCardVisibilityLastInstances(event) {
      if (this.cardVisibleNotifications || this.cardVisibleDocs) {
        this.cardVisibleNotifications = false;
        this.cardVisibleDocs = false;
      }
      this.iconLastInstances =
        "fa-solid fa-clock-rotate-left fa-beat-fade fs-4";
      setTimeout(() => {
        this.iconLastInstances = "fa-solid fa-clock-rotate-left fs-4";
      }, 1500);
      this.cardVisibleLastInstances = !this.cardVisibleLastInstances;
      if (this.cardVisibleLastInstances) {
        const iconRect = event.target.getBoundingClientRect();
        this.cardPositionLastInstances = {
          top: iconRect.top + window.scrollY - 310,
          right: iconRect.right + window.scrollX - 330,
        };
      }
    },
    toggleCardVisibilityNotifications(event) {
      if (this.cardVisibleLastInstances || this.cardVisibleDocs) {
        this.cardVisibleLastInstances = false;
        this.cardVisibleDocs = false;
      }
      this.iconNotifications = "fa-solid fa-bell fa-shake fs-4";
      setTimeout(() => {
        this.iconNotifications = "fa-solid fa-bell fs-4";
      }, 1000);
      this.cardVisibleNotifications = !this.cardVisibleNotifications;
      if (this.cardVisibleNotifications) {
        const iconRect = event.target.getBoundingClientRect();
        this.cardPositionNotifications = {
          top: iconRect.top + window.scrollY - 310,
          right: iconRect.right + window.scrollX - 220,
        };
      }
    },
    toggleCardVisibilityDocs(event) {
      if (this.cardVisibleNotifications || this.cardVisibleLastInstances) {
        this.cardVisibleNotifications = false;
        this.cardVisibleLastInstances = false;
      }
      this.iconDocs = "fa-solid fa-file-lines fa-bounce fs-4";
      setTimeout(() => {
        this.iconDocs = "fa-solid fa-file-lines fs-4";
      }, 1000);
      this.cardVisibleDocs = !this.cardVisibleDocs;
      if (this.cardVisibleDocs) {
        const iconRect = event.target.getBoundingClientRect();
        this.cardPositionDocs = {
          top: iconRect.top + window.scrollY - 310,
          right: iconRect.right + window.scrollX - 440,
        };
      }
    },
    toggleIconTheme(event) {
      if (this.iconTheme == "fa-solid fa-sun fs-4") {
        this.iconTheme = "fa-solid fa-sun fa-flip fs-4";

        setTimeout(() => {
          this.iconTheme = "fa-solid fa-moon fs-4";
        }, 1000);
      } else if (this.iconTheme == "fa-solid fa-moon fs-4") {
        this.iconTheme = "fa-solid fa-moon fa-flip fs-4";

        setTimeout(() => {
          this.iconTheme = "fa-solid fa-sun fs-4";
        }, 1000);
      }
    },
    handleClickOutside(event) {
      if (!this.$refs.sidebar.contains(event.target)) {
        this.cardVisibleLastInstances = false;
        this.cardVisibleNotifications = false;
        this.cardVisibleDocs = false;
      }
    },
    adaptNotifications() {
      this.notifications = this.notification;
      let newNotifications = {};

      for (let notif of this.notifications) {
        if (!Object.keys(newNotifications).includes(notif.title)) {
          newNotifications[notif.title] = {
            title: notif.title,
            description: notif.description,
            date: notif.date,
          };
        }
      }
      this.notifications = Object.values(newNotifications);
      console.log("Notifications:", this.notifications);
    },
    adaptInstances() {
      this.lastInstances = this.lastInstancesProp;
      let newInstances = {};

      for (let instances of this.lastInstances) {
        if (!Object.keys(newInstances).includes(instances.context)) {
          newInstances[instances.context] = {
            context: instances.context,
            url: instances.url,
            instanceid: instances.instanceid,
            ts: instances.ts,
            routerpath: instances.routerpath,
            favbit: instances.favbit,
            menutype: instances.menutype,
            entryid: instances.entryid,
          };
        }
      }
      this.lastInstances = Object.values(newInstances);
      console.log("LastInstances:", this.lastInstances);
    },
    adaptDocuments() {
      this.documents = this.documentsProp;
      let newDocuments = {};

      for (let docs of this.documents) {
        if (!Object.keys(newDocuments).includes(docs.requestid)) {
          newDocuments[docs.requestid] = {
            requestid: docs.requestid,
            userid: docs.userid,
            requestts: docs.requestts,
            form: docs.form,
            id: docs.id,
            filter: docs.filter,
            format: docs.format,
            uuid: docs.uuid,
            generationts: docs.generationts,
            formdata: docs.formdata,
            reference: docs.reference,
            requesthost: docs.requesthost,
            reportname: docs.reportname,
          };
        }
      }
      this.documents = Object.values(newDocuments);
      console.log("Documents:", this.documents);
    },
  },
  mounted() {
    this.adaptNotifications();
    this.adaptInstances();
    this.adaptDocuments();
    document.addEventListener("click", this.handleClickOutside);
  },
  beforeDestroy() {
    document.removeEventListener("click", this.handleClickOutside);
  },
};
</script>


