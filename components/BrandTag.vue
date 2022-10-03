<template>
  <div
    class="bg-offWhite opacity-90 flex items-center px-3 py-1 rounded-lg shadow-lg"
  >
    <p class="font-lora md:text-base text-sm text-gray-600">Powered By</p>
    <a href="https://magic.app/">
      <magik-logo class="invert md:h-5 md:-mx-3 h-4 -mx-5" />
    </a>
  </div>
</template>

<script>
import MagikLogo from "./MagikLogo.vue";
export default {
  components: { MagikLogo },
};
</script>

<style></style>
