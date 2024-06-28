<template>
  <div class="sidebar-logo-container border-separator">
    <div class="mx-auto">
      <div class="w-100 d-flex justify-content-center align-items-center">
        <img
          v-if="visible"
          alt="Logo"
          class="sidebar-logo-maxImg mt-3"
          :src="maxImg"
        />
        <img
          v-else
          alt="Logo"
          class="sidebar-logo-minImg"
          :src="minImg"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SideBarLogo",
  props: ["visible", "action", "maxImg", "minImg"],
};
</script>


